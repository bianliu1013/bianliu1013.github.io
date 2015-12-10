title:  Validate Binary Search Tree!
date:   2015-05-19 11:23:32
categories: algothrim
tags: algothrim
---

 Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
    
The right subtree of a node contains only nodes with keys greater than the node's key.

Both the left and right subtrees must also be binary search trees.

----------
confused what "{1,#,2,3}" means? > read more on how binary tree is serialized on OJ.

OJ's Binary Tree Serialization:

The serialization of a binary tree follows a level order traversal, where '#' signifies a path terminator where no node exists below.

Here's an example:

          1
         / \
        2   3
           /
          4
           \
            5
     


The above binary tree is serialized as "{1,2,3,#,#,4,#,#,5}". 



### code:

``` c

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
  
    bool checkLeftSubTree(TreeNode* left, int value) {
        if (NULL == left) {
            return true;
        }
        if (left->val >= value) {
            return false;
        }
        if (!checkLeftSubTree(left->left, value)) {
            return false;
        }
        if (!checkLeftSubTree(left->right, value)) {
            return false;
        }
        return true;
    }

    bool checkRightSubTree(TreeNode* right, int value) {
        if (NULL == right) {
            return true;
        }
        if (right->val <= value) {
            return false;
        }
        if (!checkRightSubTree(right->left, value)) {
            return false;
        }
        if (!checkRightSubTree(right->right, value)) {
            return false;
        }
        return true;
    }

    bool isValidBST(TreeNode* root) {
        if (NULL == root) {
            return true;
        }
        if (false == checkLeftSubTree(root->left, root->val))
            return false;
        if (false == checkRightSubTree(root->right, root->val))
            return false;

        if (!isValidBST(root->left)) {
            return false;
        }
        if (!isValidBST(root->right)) {
            return false;
        }

        return true;
    }
};

```