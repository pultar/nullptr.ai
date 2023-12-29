+++
date = 2023-01-30T12:00:00Z
lastmod = 2023-01-30T12:00:00Z
author = "default"
title = "3. Longest Substring Without Repeating Characters"
subtitle = "Solving the problem of finding the longest substring without repeating characters in C++."
tags = ["string", "sliding window", "array", "hashset"]
categories = ["c++", "algorithms", "data structures"]
maxWidthTitle = "max-w-4xl"
maxWidthFeature = "max-w-4xl"
maxWidthContent = "max-w-4xl"
math = true
+++

## Problem Statement

The problem is taken from [LeetCode](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/).

## Initial Solution: Hash Set and Sliding Window

Instead of going through the `std::string` multiple times, we use the **sliding window** technique. We keep track of a global `max`, as well as two pointers `low` and `high`, all initialized to `0`. Characters seen already will be stored in a `std::unordered_set<char>`. We iterate in a while loop with the condition `high < s.size()`. We do not use a for loop as we might enter the loop multiple times with identical `end` pointer in case we need to erase multiple characters from the `set`. In each iteration we check if the character associated with the `high` pointer is already in the `set`. If so, we add it, increase the pointer by `1`, and calculate the global `max` as `std::max` of previous `max` and the difference `high - low`. In case find the current character in the `set`, we erase it from the `set` and increase the `low` pointer. This branch of the code may be executed multiple times with the same `high` pointer. Later, we take a refined approach to tackle this redudancy in checks. Here is the code:

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        // sliding window and std::unordered_set
        std::unordered_set<char> set;
        int max = 0;
        auto low = 0, high = 0;
        while (high < s.size()) {
            auto result = set.find(s[high]);
            if (result == set.end()) {
                set.insert(s[high]);
                high++;
                max = std::max(max, high - low);
            }
            else {
                set.erase(s[low]);
                low++;
            }
        }
        return max;
    }
};
```

## Improved Solution

We can save the overhead associated with `std::unordered_set` by using a `std::vector<bool>` instead. We set the size of the `std::vector` large enough to provide space for all ASCII characters (18 or 256). The code remains unchanged otherwise:

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        // subsitute std::unordered_set for std::vector
        std::vector<bool> set(128);
        int max = 0;
        auto low = 0, high = 0;
        while (high < s.size()) {
            if (!set[s[high]]) {
                set[s[high]] = true;
                high++;
                max = std::max(max, high - low);
            }
            else {
                set[s[low]] = false;
                low++;
            }
        }
        return max;
    }
};
```

## Further Refinement

We can improve the running time from $2N$ to $N$ if we store the `idx` of each character in the `set` instead of a plain bool, which only marks the presence or absence of a character. In each iteration, we calculate `low` as the `std::max` of the previous `low` and the stored `idx` of the current character. The global `max` is calculated as the `std::max` of the old value for `max` and the difference `high - low`. The position of the current character is saved in the `std::vector`. In order to avoid a length of `0` for a string of 1 character, we start `low` from `-1`, similarly to the default values in the `set.` This approach removes the redundant computations mentioned in the initial solution. Here is the code:

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        // set stores index of character
        std::vector<int> set(128, -1);
        auto max = 0;
        for (auto low = -1, high = 0; high < s.size(); ++high) {
            auto current = s[high];
            low = std::max(low, set[current]);
            max = std::max(max, high - low);
            set[current] = high;
        }
        return max;
    }
};
```
