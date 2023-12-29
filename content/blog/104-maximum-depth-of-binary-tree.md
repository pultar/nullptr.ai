+++
date = 2023-01-23T13:00:00Z
lastmod = 2023-01-23T13:00:00Z
author = "default"
title = "104. Maximum Depth of Binary Tree"
subtitle = "Solving the problem of finding the maximum depth of a binary tree in C++."
tags = ["binary tree", "dfs", "bfs"]
categories = ["c++", "algorithms", "data structures"]
maxWidthTitle = "max-w-4xl"
maxWidthFeature = "max-w-4xl"
maxWidthContent = "max-w-4xl"
math = true
+++

## The Problem Statement

The problem is taken from [LeetCode](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/).

## First Solution: Recursive Depth-First Search

I found it most straightforward to run a **recursive depth-first search** (DFS) on the tree. The intuition is that the maximum depth of any node is simply the maximum depth of either the left or the right half of the tree + `1`. The base case for our recursive call are `nullptr` nodes which have a depth of `0`. Here is the code that implements these ideas:

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    
    int maxDepth(TreeNode* root) {
        // base case
        if (root == nullptr) {
            return 0;
        }
        auto left = maxDepth(root->left);
        auto right = maxDepth(root->right);
        auto max = 1 + std::max(left, right);
        return max;
    }

};
```

The time complexity is $O(N)$ as each node is visited once. The space complexity is dependent on the height of the tree. For a balanced binary search tree, the space complexity is $O(log N)$. For a completely unbalanced binary tree, space complexity is $O(N)$.

## Second Solution: Iterative Depth-First Search

We can mimic the call stack of the recursive DFS algorithm in solution #1 with a `std::stack` data structure and an iterative approach. We simply initialize a `std::stack<std::pair<TreeNode*,int>>` that holds all nodes to be processed with their respective depth. In each iteration, we verify if the depth of the current node is larger than the maximum depth found so far. We also add the children of the last node (FIFO) to the stack. The depth associated with the children is simply the depth found for the parent + `1`. The algorithm will run until all nodes have been processed.

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* root) {
        // iterative DFS
        std::stack<std::pair<TreeNode*, int>> stack;
        stack.push({root, 1});
        auto result = 0;
        while(!stack.empty()) {
            auto[item, depth] = stack.top();
            stack.pop();
            if (item != nullptr) {
                result = std::max(result, depth);
                stack.push({item->right, depth + 1});
                stack.push({item->left, depth + 1});
            }
        }
            
        return result;
    }

};
```

Time complexity again is $O(N)$ as all nodes are processed. Space complexity again depends on the layout of the tree. For perfectly balanced binary search trees, we obtain $O(log N)$. For completely unbalanced trees, space complexity is $O(N)$.

## Third Solution: Iterative Breadth-First Search

Alternatively, we can run a **breadth-first search** (BFS). Instead of processing each path individually and comparing results afterward as we did with DFS, we go through the tree layer by layer while incrementing a variable `count`. We terminate once there are no more layers. BFS algorithms typically require a queue that keeps track of elements to be processed. After checking the base case of an empty tree, we add `root` to a `std::queue<TreeNode*>`. In each iteration, we increment `count` and pop all elements into a temporary queue. Subsequently, we add all children of the nodes in the temporary queue to the main queue. Here is the code for BFS:

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:

    int maxDepth(TreeNode* root) {
        // iterative BFS
        if (root == nullptr) {
            return 0;
        }
        std::queue<TreeNode*> queue;
        int count = 0;
        queue.push(root);
        while (!queue.empty()) {
            // pop all items into temp queue
            count++;
            std::queue<TreeNode*> items;
            while (!queue.empty()) {
                auto item = queue.front();
                queue.pop();
                items.push(item);
            }
            // check for children
            while (!items.empty()) {
                auto item = items.front();
                items.pop();
                if (item->left != nullptr) {
                    queue.push(item->left);
                }
                if (item->right != nullptr) {
                    queue.push(item->right);
                }
            }
        }
        return count;
    }

};
```

Time complexity is identical to previous solutions. As all nodes are visited once, run time scales with $O(N)$. Space complexity would be $O(N)$ as we will hold $N/2$ elements in the queue during the last layer.
