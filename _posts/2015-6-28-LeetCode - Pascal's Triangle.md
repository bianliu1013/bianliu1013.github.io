---
layout: post
title: leetcode -  Pascal's Triangle!
date:   2015-06-28 11:23:32
categories: leetcode c++
---


Given numRows, generate the first numRows of Pascal's triangle.


For example, given numRows = 5,

Return 


---
    [
        [1],
       [1,1],
      [1,2,1],
     [1,3,3,1],
    [1,4,6,4,1]
    ]
    
---



### code:
<pre><code>
class Solution {
  public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> a;
        if (numRows == 0) {
            return a;
        }

        if (numRows == 1) {
            a.push_back({ 1 });
            return a;
        } else {
            a.push_back({ 1 });
            for (int i = 1; i < numRows; i++) {
                vector<int> array_i = { 1 };
                for (int j = 1; j <= i - 1; j++) {
                    vector<int>& last_array = a[i - 1];
                    int j_value = last_array[j - 1] + last_array[j];
                    array_i.push_back(j_value);
                }
                array_i.push_back(1);
                a.push_back(array_i);
            }
        }

        return a;
    }
};
</code></pre>