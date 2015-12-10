title:   Binary Tree Maximum Path Sum!
date:   2015-06-23 11:23:32
categories: algothrim
tags: algothrim
---


 Given a binary tree, find the maximum path sum.


The path may start and end at any node in the tree.


For example:


Given the below binary tree, 


---
        1
    /       \
    2       3
    
---

return <font color=red>6</font>


### code:

``` c


class Solution {
  public:
    int maxSum;

    Solution() {
        maxSum = INT_MIN;
    }
    int maxDeepSum(TreeNode* root) {
        if (root == NULL) {
            return 0;
        } else {
            int l = maxDeepSum(root->left);
            int r = maxDeepSum(root->right);
            int v = root->val + max(0, (l > r ? l : r));
            maxSum = max(maxSum, max(v, root->val + l + r));
            return v;
        }
    }

    int maxPathSum(TreeNode* root) {
        if (root == NULL) {
            return 0;
        }

        int v = root->val + max(0, maxDeepSum(root->left)) + max(0, maxDeepSum(root->right));
        maxSum = v > maxSum ? v : maxSum;
        return maxSum;
    }
};



```