title: Maximum Depth of Binary Tree!
date:   2015-05-17 11:23:32
categories: algothrim
tags: algothrim
---

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.



----------

### code

```c++

/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
 
class Solution {
public:
    int maxDepth(TreeNode *root) {
        if(root == NULL)
            return 0;
        if(root->left == NULL && root->right == NULL)
            return 1;
        int lMax = maxDepth(root->left);
        int rMax = maxDepth(root->right);
        return 1 + (lMax >= rMax? lMax: rMax);
    }
};

```