+++
date = 2023-01-23T20:59:00Z
lastmod = 2023-01-20T21:59:00Z
author = "default"
title = "217. Contains Duplicate"
subtitle = "Solving the problem of finding duplicate elements in an array in C++."
tags = ["array", "set"]
categories = ["c++", "algorithms", "data structures"]
maxWidthTitle = "max-w-4xl"
maxWidthFeature = "max-w-4xl"
maxWidthContent = "max-w-4xl"
math = true
+++

## The Problem Statement

This problem is taken from [LeetCode](https://leetcode.com/problems/contains-duplicate/description/).

## First Solution: std::unordered_set

In order to avoid quadratic time complexity resulting from comparing each pair of numbers, we can use a `std::unordered_set<int>`. We iterate once through all elements and check if the element in already in our `set`. If yes, we can return `true`. If not, we add the element to our `set` and continue. In case we make it through all elements and have not found a duplicate in the `set`, we return `false`.

```c++
class Solution {
public:

    bool containsDuplicate(vector<int>& nums) {
        // unordered_set
        std::unordered_set<int> set;
        for (const auto e : nums) {
            auto result = set.find(e);
            if (result != set.end()) {
                return true;
            }
            else {
                set.insert(e);
            }
        }
        return false;
    }
};
```

The algorithm has $O(N)$ time complexity and $O(N)$ space complexity as all elements have to be processed and potentially stored in the `set`. It is important to take the member function `find` of `std::unordered_set` and not the more generic `std::find` to achieve constant lookup time.

## Second Solution: Sorting the Array First

A second approach is to first sort the array. Subsequenly, potential duplicates will be next to each other and we only have to process each element of the array once. For smaller arrays, this approach can be faster than the first solution.

```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        // sorting
        std::sort(nums.begin(), nums.end());
        for (auto i = 0; i < nums.size() - 1; ++i) {
            if (nums[i] == nums[i + 1]) {
                return true;
            }
        }
        return false;
    }

};
```

Sorting takes $O(N \times log N)$ time complexity. Space complexity is $O(1)$ since we sorted the `std::vector` inplace.
