title:  Combinations!
date:   2015-06-28 11:23:32
categories: algothrim
tags: algothrim
---


 Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.


For example,

If n = 4 and k = 2, a solution is: 


Return 


---
    [
    [2,4],
    [3,4],
    [2,3],
    [1,2],
    [1,3],
    [1,4],
    ]
    
---



### code:

``` c



class Solution {
  public:
    vector<vector<int>> combine_sub(int b, int e, int k) {
        vector<vector<int>> a;
        if (k == 0) {
            return a;
        }

        if (k == 1) {
            for (int i = b; i <= e; i++) {
                vector<int> a_i = { i };
                a.push_back(a_i);
            }
            return a;
        }

        for (int i = b; i <= e - k + 1; i++) {
            vector<vector<int>> a_left = combine_sub(i + 1, e, k - 1);
            if (a_left.size() > 0) {
                for (auto j : a_left) {
                    vector<int> ai = { i };
                    for (auto l : j) {
                        ai.push_back(l);
                    }
                    a.push_back(ai);
                }
            }
        }
        return a;
    }


    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> a = combine_sub(1, n, k);
        return a;
    }
};



```