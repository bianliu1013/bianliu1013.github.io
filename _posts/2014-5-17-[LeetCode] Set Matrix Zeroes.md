---
layout: post
title: [leetcode] - Set Matrix Zeroes!
---

 Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in place.
click to show.
**Follow up**:

Did you use extra space?

A straight forward solution using O(mn) space is probably a bad idea.

A simple improvement uses O(m + n) space, but still not the best solution.
Could you devise a constant space solution?

----------

### code

```c++

class Solution {
  public:
    void setZeroes(vector<vector<int>>& matrix) {
        if (matrix.size() == 0) {
            return;
        }
        vector<int> hList;
        vector<int> vList;
        hList.resize(matrix.size(), 0);
        vList.resize(matrix[0].size(), 0);

        for (auto i = 0; i < matrix.size(); i++) {
            std::vector<int>& it = matrix[i];
            for (auto j = 0; j < it.size(); j++) {
                int value = it[j];
                if (value == 0) {
                    hList[i] = 1;
                    vList[j] = 1;
                }
            }
        }

        for (auto i = 0; i < matrix.size(); i++) {
            std::vector<int>& it = matrix[i];
            for (auto j = 0; j < it.size(); j++) {
                if (hList[i] == 1 || vList[j] == 1) {
                    it[j] = 0;
                }
            }
        }
    }
};

```