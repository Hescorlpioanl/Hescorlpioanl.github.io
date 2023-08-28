---
title: "Problem Solving Notes"
classes: wide
header:
  teaser: /assets/images/Blogs/Learning-Notes/problem_solving.jpg
ribbon: ForestGreen
description: "My Notes that I take during solving leetcode problems and other platforms"
categories:
  - Notes
tags:
  - leetcode
  - problem solving
toc: true
---

# 1- Dutch National Flag algorithm (DFA)

**Problem - Leetcode 75. Sort Colors**
```
Given an array nums with n objects colored red, white, or blue.
sort them `in-place` so that objects of the same color are adjacent,
with the colors in the order red, white, and blue.

We will use the integers 0, 1, and 2 to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.
```
**Examples**

```
[1]
Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]

[2]
Input: nums = [2,0,1]
Output: [0,1,2]
```

**Code**
```java
class Solution {
    public void sortColors(int[] nums) {
        // in this algorithm we start looping over the array elemments.
        // we put all 0's at the start pointer.
        // aall 1's at the middle of the array.
        // aall 2's at the end of the array.
        // The worst case for this algorithm is O(n).
        int start = 0;
        int end = nums.length - 1;
        int mid = 0;
        while (p <= end) {
            if (nums[mid] == 0) {
                swap(nums, mid, start);
                start++;
                mid++;
            } else if (nums[mid] == 1) {
                mid++;
            } else {
                swap(nums, mid, end);
                end--;
            }
        }
    }

    public void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

---
# 2- Getting the sum of numbers fromm 0 to N

This code helps me to get the missing numbers from an array of unique integers with missing numbers.

**Code**
```java
    int n = nums.length;
    // calculate the sum of all the numbers in the array. O(1)
    int numsSum = n * (n + 1) / 2;
```
---