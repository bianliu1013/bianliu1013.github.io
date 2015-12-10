title: Move Zeroes
date: 2015-12-10 10:52:10
tags: algothrim
---

 
 Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3, 12, 0, 0].

Note:

    You must do this in-place without making a copy of the array.
    Minimize the total number of operations.



### code:
```cplusplus
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        if(nums.size() <= 1)
            return;

        int f = 0;
        int s = 1;
        int temp = 0;

        while(s < nums.size() && f < nums.size())
        {
            // compare
            if(nums[f] == 0){
                if(nums[s] != 0){
                    temp = nums[f];
                    nums[f] = nums[s];
                    nums[s] = temp;
                    f += 1;
                    s += 1;
                }
                else{  // 0, 0, 1
                    s += 1;
                }
            }
            else{
                f += 1;
                s += 1;
            }
        }
    }
};
```