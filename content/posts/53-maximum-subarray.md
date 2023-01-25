+++
date = 2023-01-24T13:00:00Z
lastmod = 2023-01-24T13:00:00Z
author = "default"
title = "53. Maximum Subarray"
subtitle = "Solving the problem of finding the maximum subarray in C++."
tags = ["array", "dynamic programming"]
categories = ["c++", "algorithms", "data structures"]
maxWidthTitle = "max-w-4xl"
maxWidthFeature = "max-w-4xl"
maxWidthContent = "max-w-4xl"
math = true
+++

## The Problem Statement

This problem is taken from [LeetCode](https://leetcode.com/problems/maximum-subarray/description/).

## The Solution - Kadane's Algorithm

It is very easy to end up with a solution that is $O(N^2)$ or $O(N^3)$ in time complexity by comparing all possible subarrays. The challenge is to find a solution that is only $O(N)$. In the following solution, we will only go once over all elements. The intuition for the solution is the fact that we can discard any subarray that is followed by a subarray with a higher sum. For example, if we consider the sequence `[-2, 1, -3, 4]`, the subarray `[1]` has a higher sum than the initial subarray `[-2, 1]`. All subarrays that follow from `[-2, 1]` will consequently also be smaller than equivalent subarrays that are derived from `[1]` alone. We can thus safely stop computing solutions for the former. The following code implements this concept:

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        // Kadane's algorithm, dynamic programming
        int max = nums.front();
        int current = 0;
        for (auto num : nums) {
            current = std::max(num, current + num);
            max = std::max(current, max);
        }
        return max;
    }
};
```

We keep track of the global maximum `max` and the sum of the currently considered subarray `current`. At each step, we compute if the subarray resulting from `current + num` is bigger than `num` alone. Subsequenly, we check if we have reached a new global maximum `max`. As a result, we achieve $O(N)$ time complexity and $O(1)$ space complexity. The algorithm is called [Kadane's algorithm](https://en.wikipedia.org/wiki/Maximum_subarray_problem#Kadane's_algorithm) and is an example of **dynamic programming**.
