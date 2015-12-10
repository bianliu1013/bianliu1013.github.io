title:  longest increasing subsequence
date:   2015-11-6 11:23:32
categories: algothrim
tags: algothrim
---


Given an unsorted array of integers, find the length of longest increasing subsequence.

For example,
Given [10, 9, 2, 5, 3, 7, 101, 18],
The longest increasing subsequence is [2, 3, 7, 101], therefore the length is 4. Note that there may be more than one LIS combination, it is only necessary for you to return the length.

Your algorithm should run in O(n2) complexity.

Follow up: Could you improve it to O(n log n) time complexity? 

### code:
```cplusplus
class Solution {
  public:
    int lengthOfLIS(vector<int>& nums) {
        int max_length = 1;
        if (nums.size() <= 1) {
            return nums.size();
        }

        std::vector<int> matrics(nums.size(), 1);
        for (int i = 0; i < nums.size(); i++) {
            for (int j = i + 1; j < nums.size(); j++) {
                if (nums[j] > nums[i]) {
                    if (nums[j] > nums[j - 1] && nums[j - 1] > nums[i]) {
                        matrics[j] = matrics[j - 1] + 1;
                        max_length = max(max_length, matrics[j]);
                    } else {
                        matrics[j] = max(matrics[i] + 1, matrics[j]);
                        max_length = max(max_length, matrics[j]);
                    }
                } else {
                }
            }
        }

        return max_length;
    }
};
```