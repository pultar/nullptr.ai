+++
date = 2023-01-23T09:00:00Z
lastmod = 2023-01-23T09:00:00Z
author = "default"
title = "543. Diameter of Binary Tree"
subtitle = "Solving the problem of finding the diamter of a binary tree in C++."
tags = ["binary tree", "dfs"]
categories = ["c++", "algorithms", "data structures"]
maxWidthTitle = "max-w-4xl"
maxWidthFeature = "max-w-4xl"
maxWidthContent = "max-w-4xl"
math = true
+++

## The problem statement

The problem is taken from [LeetCode](https://leetcode.com/problems/diameter-of-binary-tree/description/).

## The solution

We can write a helper function `int dfs(TreeNode* root, int& max)` that *recursively* computes the height of all possible subtrees. This algorithm is called **depth-first search** and quite common in tree and graph problems. The diameter of these subtrees through their respective *root node* will simply be the **combined heights** of the left and the right subtrees. Comparison of the diameter computed for the current subtree with a global variable `max` allows the find the global maximum diameter. The base case we have to consider is a node `root` of value `nullptr`, which will we simply assign a height of 0.

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
    
    int diameterOfBinaryTree(TreeNode* root) {
        auto max = 0;
        auto height = dfs(root, max);
        return max;
    }

    int dfs(TreeNode* root, int& max) { // returns the height of the subtree starting from root
        // base case
        if (root == nullptr) {
            return 0;
        }
        // dfs
        auto left = dfs(root->left, max);
        auto right = dfs(root->right, max);
        auto diameter = left + right;
        max = std::max(diameter, max);
        // height is max height of subtree + node itself
        return 1 + std::max(left, right);
    }
};
```

Note that [some people](https://www.youtube.com/watch?v=bkxqA8Rfv04) prefer assigning nodes of the base case `nullptr` a height of –1. We can compensate for this relative offset by adding 2 for our `diameter` variable. Instead of passing a reference to our `max` variable, we could also have the function `dfs` return a `std::pair<int,int>` consisting of the height and the max diameter.
