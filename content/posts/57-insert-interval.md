+++
date = 2023-01-25T11:00:00Z
lastmod = 2023-01-25T11:00:00Z
author = "default"
title = "57. Insert Interval"
subtitle = "Solving the problem of inserting an interval into a sorted array in C++."
tags = ["array", "intervals"]
categories = ["c++", "algorithms", "data structures"]
maxWidthTitle = "max-w-4xl"
maxWidthFeature = "max-w-4xl"
maxWidthContent = "max-w-4xl"
math = true
+++

## The Problem Statement

This problem is taken from [LeetCode](https://leetcode.com/problems/insert-interval/description/).

## The Solution

We will allocate a new `std::vector` and reserve a capacity of `intervals.size() + 1`. We loop over all `intervals` and during each step check if the end of the `newInterval` is smaller than the head of the current interval. If so, we can safely insert `newInterval` in front without worrying about overlap. We will also append all remaining elements of `intervals` to our `res` vector and return. While we iterate through all `intervals`, we will also check if the head of the `newInterval` is bigger than the tail of the current `interval`. If the condition is fullfilled, we add `newInterval` to `res` but do not return yet as there might be overlaps with the remaining elements of `intervals`. The third clause in our `if`-`else` block concerns merging overlapping intervals. In case a collision is detected, we update head and tail of `newInterval` with `std::min(newInterval[0]), intervals[i][0]` and `std::max(newInterval[1], intervals[i][1])`. After the loop has finished, we know that `newInterval` has the right bounces but is not appended yet as the first branch of our `if`-`else` block has never executed. We thus push back `currentInterval` and return `res`. Here is the code:

```c++
class Solution {
public:

    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        std::vector<std::vector<int>> res;
        res.reserve(intervals.size() + 1);
        for (int i = 0; i < intervals.size(); ++i) {
            if (newInterval[1] < intervals[i][0]) {
                // check if new interval can fit in front
                res.push_back(newInterval);
                res.insert(res.end(), intervals.cbegin() + i, intervals.cend());
                return res;
            }
            else if (newInterval[0] > intervals[i][1]) {
                // check if new interval goes after current item
                res.push_back(intervals[i]);
            }
            else {
                // merge interval with current item
                newInterval = {std::min(newInterval[0], intervals[i][0]), std::max(newInterval[1], intervals[i][1])};
            }
        }
        res.push_back(newInterval);
        return res;
    }
};
```

## Notes

The algorithm has $O(N)$ time complexity and $O(1)$ space complexity as we need to process the list once and aside from the `res` vector do not allocate any more space. Typically, allocating space for the result of a task is not considered to affect space complexity. In case we are concerned with the new allocation of `res`, we could also insert inplace after performing a linear or binary search on `invervals`. Afterwards, we will need to merge the interval to avoid overlap and erase duplicate elements. This process will naturally trigger lots of reallocations and is thus potentially less efficient than the solution presented. We would also need to be very careful of iterator invalidation.
