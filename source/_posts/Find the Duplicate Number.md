
 Given an array nums containing n + 1 integers where each integer is between 1 and n (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

Note:

    You must not modify the array (assume the array is read only).
    You must use only constant, O(1) extra space.
    Your runtime complexity should be less than O(n2).
    There is only one duplicate number in the array, but it could be repeated more than once.



class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int low = 0;
        int high = nums.size() - 1;
        int mid = 0;

        while(low < high)
        {
            mid = (low + high) / 2;
            int small_count = 0;
            for(size_t i = 0; i < nums.size(); i++)
            {
                if(nums[i] <= mid)
                    small_count++;
            }
            if(small_count > mid)  // duplicated num exist in lower side
                high = mid;
            else
                low = mid + 1;
        }

        return low;
    }
};