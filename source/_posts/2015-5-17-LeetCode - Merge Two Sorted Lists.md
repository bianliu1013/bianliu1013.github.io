title:  Merge Two Sorted Lists!
date:   2015-05-17 11:23:32
categories: algothrim
tags: algothrim
---

 Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.


 
----------

### code

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *mergeTwoLists(ListNode *l1, ListNode *l2) {
        ListNode* p = NULL;
        ListNode* p1 = l1;
        ListNode* p2 = l2;
        
        if(l1 == NULL)
             return l2;
        if(l2 == NULL)
            return l1;
            
            // head
        if(l1->val <= l2->val)
        {
            p = p1;
            p1 = p1->next;
        }
        else
        {
            p = p2;
            p2 = p2->next;
        }
        
        ListNode* p_head = p;
        while(p1 != NULL || p2 != NULL)
        {
            if(p1 == NULL){
                p->next = p2;
                return p_head;
            }
            if(p2 == NULL){
                p->next = p1;
                return p_head;
            }
                
            if(p1->val <= p2->val)
            {
                p->next = p1;
                p1 = p1->next;
                p = p->next;
            }
            else
            {
                p->next = p2;
                p2 = p2->next;
                p = p->next;           
            }
        }
        
        return p_head;
    }
};

```