---
layout: post
title: [leetcode] - Search Insert Position!
---

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

Here are few examples.
[1,3,5,6], 5 → 2
[1,3,5,6], 2 → 1
[1,3,5,6], 7 → 4
[1,3,5,6], 0 → 0 
 
----------

### code

```c++

class Solution {
  public:
    int searchInsert(vector<int>& nums, int target) {
        if (nums.size() == 0) {
            return 0;
        }
        if (nums.at(nums.size() - 1) < target) {
            return nums.size();
        }

        int i = 0;
        while (i < nums.size()) {
            if (target <= nums[i]) {
                return i;
            }
            i++;
        }

        return nums.size();
    }
};

```