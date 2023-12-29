+++
date = 2023-01-25T11:00:00Z
lastmod = 2023-01-25T11:00:00Z
author = "default"
title = "542. 01 Matrix"
subtitle = "Solving the problem of computing nearest distances in a matrix in C++."
tags = ["array", "bfs", "dynamic programming"]
categories = ["c++", "algorithms", "data structures"]
maxWidthTitle = "max-w-4xl"
maxWidthFeature = "max-w-4xl"
maxWidthContent = "max-w-4xl"
math = true
+++

## Problem Statement

The problem is taken from [LeetCode](https://leetcode.com/problems/01-matrix/description/).

## First Solution - Breadth-First Search

Computing closest distances in a matrix should always trigger **BFS**. We initialize a `std::vector` for the computed distances results. There is two ways we can go about performing BFS: either we start from all `1's` and try to find the closest `0`, or we start from all `0's` and try to find all `1's`. It turns out that the latter approach is computationally more efficient. The intuition is that every BFS search starting from a `1` will only yield one `0`, while a BFS search started from a `0` can yield in more than one `1` being processed. 


### BFS - Searching from Source to Target

Here is the code for BFS starting from all `1's`:

```c++
class Solution {
public:

    vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
        // bfs, start from 1's
        auto const m = mat.size();
        auto const n = mat[0].size();
        std::vector<std::vector<int>> res(m);
        for (auto& col : res) {
            col.resize(n);
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                res[i][j] = mat[i][j] == 0 ? 0 : bfs(mat, {i, j});
            }
        }
        return res;
    }

    int bfs(vector<vector<int>>& mat, std::pair<int,int> root) {
        // updates a single field
        int distance = 0;
        std::vector<std::pair<int,int>> directions = {
            {-1, 0}, {1, 0}, {0, -1}, {0,  1}
        };
        auto const m = mat.size();
        auto const n = mat[0].size();
        std::queue<std::pair<int,int>> queue;
        queue.push(root);
        while (!queue.empty()) {
            // get all children
            std::vector<std::pair<int,int>> items;
            auto size = queue.size();
            for (auto k = 0; k < size; ++k) {
                auto[i, j] = queue.front();
                if (mat[i][j] == 0) {
                    return distance;
                }
                queue.pop();
                items.push_back({i, j});
            }
            
            // not found in the current layer
            distance++;
            for (auto [i, j] : items) {
                for (auto d : directions) {
                    auto ni = i + d.first;
                    auto nj = j + d.second;
                    if (ni >= 0 && ni < m && nj >= 0 && nj < n) {
                        queue.push({ni, nj});
                    }
                }
            }
        }
        return distance;
    }

};
```

Note that we will always add all neighbors to the `queue` during BFS if there are within bounds. They could be either `0`, in which case we have found a solution, or not `0`. If there are not `0`, they might still connect `root` to the nearest `0` and we have to explore the branch. We also have to keep track somehow of elements of the `queue` in the current layer of the search tree and introduce a temporary `std::vector`. For me, this solution passes 48 out of 50 tests and times out for the last two. 


### BFS - Searching from Target to Source

Here is the code for the solution that starts exploring from all `0's`. After I wrapped my head around the fact that order of search actually matters in this case, I find this solution more intuitive than the first one. We again initialize a `std::vector` for our minimum distances calculated. All elements with the trivial solution `0` are initialized to `0` and also pushed to a `queue`. Non-zero elements are labeled as `-1` in the results matrix. Subsequenly we process the queue one layer at a time. If we found a new `1` that is labelled uninitialized (`-1`), we assign the current `distance`. Afterwards, we add all neighbors within bounds that are labelled `-1`. We could leave out the comparison `res[i][j] == -1` in `if (mat[i][j] == 1 && res[i][j] == -1)` if we can guarantee that the `queue` only has unique elements. For example, we could use a `std::unordered_set` to keep track of visited nodes. Here is the code: 

```c++
class Solution {
public:

    vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
        // bfs, start from 0's
        std::vector<std::vector<int>> res;
        auto const m = mat.size();
        auto const n = mat[0].size();
        res.resize(m);
        for (auto& col : res) {
            col.resize(n);
        }
        std::queue<std::pair<int,int>> queue;
        std::vector<std::pair<int,int>> directions = {
            {-1, 0}, { 1, 0}, { 0, -1}, { 0,  1}
        };
        // start bfs from all 0's
        for (auto i = 0; i < m; ++i) {
            for (auto j = 0; j < n; ++j) {
                if (mat[i][j] == 0) {
                    res[i][j] = 0;
                    queue.push({i, j});
                }
                else {
                    res[i][j] = -1; // mark as unprocessed
                }
            }
        }
        auto dist = 0;
        while (!queue.empty()) {
            // start from all zeros
            auto size = queue.size();
            for (auto k = 0; k < size; ++k) {
                auto [i, j] = queue.front();
                queue.pop();
                if (mat[i][j] == 1 && res[i][j] == -1) {
                    res[i][j] = dist;
                }
                for (auto d : directions) {
                    auto ni = i + d.first;
                    auto nj = j + d.second;
                    if (ni >= 0 && ni < m && nj >= 0 && nj < n && res[ni][nj] == -1) {
                        queue.push({ni, nj});
                    }
                }
            }
            ++dist;
        }
        return res;
    }

};
```

The solution completes all tests successfully for me.

## Second Solution - Dynamic Programming

This problem is also very well suited for a **dynamic programming** approach. Essentially, we will compute the distance of a `1` node from a `0` node as the `std::min(a, b, c, d) + 1` where `a`, `b`, `c`, `d` are neighboring nodes. The problem is that we initially do not know the associated distances of these neighbors either. However, we can still refine our algorithm and solve this problem. We initially allocate a `std::vector` for our result matrix and initialize elements with `std::numeric_limits<int>::max()` to mark them as unprocessed. We start iterating from the top-left element until we have reached the bottom-right element. In each step, we check if the current matrix element is `0`. If so, we assign the corresponding element in the `res` matrix `0`. If not, we assign `res` for this element as the `std::min(up, left) + 1`. We could also include `right` and `down`, however, we know that these elements will always be `infinity` as they haven't been processed yet. Consequently, we might have missed shortest paths to a `0` in the first iteration. To account for this problem, we will iterate back from the bottom-right element to the top-left element. We don't need to check for `0` as in the first case and only compare the current `res` element with the `std::min(bottom, right) + 1`. We also don't need to consider values for `left` and `up` anymore as their respective minimum is already reflected in the current value of `res`. Here is the code:

```c++
class Solution {
public:

    vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
        int const BIG = std::numeric_limits<int>::max() - 1000;
        // dynamic programming
        std::vector<std::vector<int>> res;
        auto const m = mat.size();
        auto const n = mat[0].size();
        res.resize(m);
        for (auto& col : res) {
            col.resize(n);
        }
        // init to infinity
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                res[i][j] = BIG;
            }
        }

        // first round top-left to bottom-right
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (mat[i][j] == 0) {
                    res[i][j] = 0;
                }
                else {
                    auto up = (i - 1 >= 0) ? res[i - 1][j] : BIG;
                    auto left = (j - 1 >= 0) ? res[i][j - 1] : BIG;
                    res[i][j] = 1 + std::min(left, up);
                }
            }
        }

        // second round bottom-right to top-left
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                auto right = (j + 1 < n) ? res[i][j + 1] : BIG;
                auto down = (i + 1 < m) ? res[i + 1][j] : BIG;
                res[i][j] = std::min(std::min(right, down) + 1, res[i][j]);
            }
        }
        
        return res;
    }

};
```

## Notes

For decrementing loop variables, remember to **not** assign them to `size_t` or `unsigned` as there will be an overflow at `0.` When filling in `infinity` or `numeric_limits` to label unprocessed nodes, remember it might be necessary to subtract a small number to alleviate the risk of an overflow, e.g. in the line `std::min(right, down) + 1`.
