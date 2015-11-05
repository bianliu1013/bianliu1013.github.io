---
layout: post
title: leetcode -  Invert a binary tree.
date:   2015-11-6 11:23:32
categories: leetcode c++
---


---
     4
   /   \
  2     7
 / \   / \
1   3 6   9

to

     4
   /   \
  7     2
 / \   / \
9   6 3   1
    
---

Trivia:
This problem was inspired by this original tweet by Max Howell: 

### code:
<pre><code>
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(nullptr == root)
            return root;
        if(nullptr == root->left && nullptr == root->right)
            return root;

        root->left = invertTree(root->left);
        root->right = invertTree(root->right);
        TreeNode* pTmp = root->left;
        root->left = root->right; 
        root->right = pTmp;
        return root;
    }
};
</code></pre>