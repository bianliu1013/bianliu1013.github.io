title: Summary Ranges
date:   2015-12-9 11:23:32
categories: algothrim
tags: algothrim
---


 Given a sorted integer array without duplicates, return the summary of its ranges.

For example, given [0,1,2,4,5,7], return ["0->2","4->5","7"]. 



### code:
```cplusplus
class Solution {
public:
    vector<string> summaryRanges(vector<int>& nums) {
         int begin = 0;
         int end = 0;

         vector<string> v;

         if(nums.size() == 0){
             return v;
         }

         for(end = 0; end < nums.size(); ++end)
         {
             if(end == begin){
             }
            else if(nums[end] == nums[end - 1] + 1){
                //end++;
            }
            else{
                ostringstream os;
                if(begin == end - 1)
                   os << nums[begin];
                else
                   os << nums[begin] << "->" << nums[end - 1];
                v.push_back(os.str());
                begin = end;
            }
         }

         ostringstream os;
         if(begin == end - 1)
            os << nums[begin];
         else
            os << nums[begin] << "->" << nums[end - 1];

         v.push_back(os.str());

         return v;
    }
};
```