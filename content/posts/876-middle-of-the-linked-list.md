+++
date = 2023-01-23T12:00:00Z
lastmod = 2023-01-23T12:00:00Z
author = "default"
title = "876. Middle of the Linked List"
subtitle = "Solving the problem of finding the middle of a linked list in C++."
tags = ["linked list", "two pointers", "array"]
categories = ["c++", "algorithms", "data structures"]
maxWidthTitle = "max-w-4xl"
maxWidthFeature = "max-w-4xl"
maxWidthContent = "max-w-4xl"
math = true
+++

## The Problem Statement

The problem is taken from [LeetCode](https://leetcode.com/problems/middle-of-the-linked-list/description/).

## First Solution: Naïve Copy to std::vector

The underlying problem here is that linked lists don't have random access. If the data structure we operate on were a `std::vector` or `std::array`, we could simply calculate the middle via the size of the container. So why don't we just copy all elements of the list into a `std::vector`?

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        // naive approach -> copy to vector
        std::vector<ListNode*> elements;
        ListNode* current = head;
        while (current != nullptr) {
            elements.push_back(current);
            current = current->next;
        }
        return elements[elements.size() / 2];
    }
};
```

This approach has $O(n)$ run time complexity as we have to iterate once over the list and $O(n)$ space complexity as we copy each element into a `std::vector`. It's worth noting though that we only copy pointers and not the underlying data. In this particular case, the linked list stores `int` and accordingly, this difference does not change anything. However, it's conceivable that the linked list stores big objects (e.g. a 3D mesh), which we would *not* copy using this approach. In other words, it's potentially not as harmful to copy the data as the space complexity suggests. However, there is a better approach.

## Two-Pointer Running Technique

One way to avoid linear space complexity in this problem is the **two-pointer running technique**. We will instantiate two pointers named `slowRunner` and `fastRunner` that both initially point to the head node of the list. Subsequently, we will go through list once. In each iteration, `slowRunner` moves one node at a time and `fastRunner` moves two nodes at a time. Once `fastRunner` falls off the list, `slowRunner` points to the middle of the list.

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        // two-pointer running
        ListNode* slowRunner = head;
        ListNode* fastRunner = head;
        while (fastRunner != nullptr && fastRunner->next != nullptr) {
            slowRunner = slowRunner->next;
            fastRunner = fastRunner->next->next;
        }
        return slowRunner;
    }
};
```

This solution also has $O(n)$ run time complexity but only $O(1)$ space complexity.
