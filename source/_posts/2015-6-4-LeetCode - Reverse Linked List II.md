title:  Reverse a linked list!
date:   2015-06-04 11:23:32
categories: algothrim
tags: algothrim
---


 Reverse a linked list from position m to n. Do it in-place and in one-pass.


For example:

Given <font color=red>1->2->3->4->5->NULL</font>, m = 2 and n = 4,


return <font color=red>1->4->3->2->5->NULL</font>.

Note:

Given m, n satisfy the following condition:

1 ≤ m ≤ n ≤ length of list. 


----------




### code:

``` c

class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        if (NULL == head)
        {
            return head;
        }

        if (m == 1 && m == n)
        {
            return head;
        }

        ListNode* p_m = NULL;
        ListNode* p_n = NULL;
        ListNode* p_cur = head;
        ListNode* p_m_pre = p_cur;
        ListNode* p_n_next = NULL;


        int index = 1;
        bool mFound = false;
        while (true)
        {
            if (index == m)
            {
                p_m = p_cur;
                mFound = true;
            }
            if (index == n)
            {
                p_n = p_cur;
                p_n_next = p_n->next;
                break;
            }
            if (!mFound) 
            {
                p_m_pre = p_cur;
            }
            p_cur = p_cur->next;
            index++;
        }

        ListNode* pTemp = p_m;
        ListNode* pTemp2 = p_m->next;
        p_m->next = NULL;
        while (pTemp != p_n)
        {
            ListNode* pTemp3 = pTemp2->next;
            pTemp2->next = pTemp;
            pTemp = pTemp2;
            pTemp2 = pTemp3;
        }

        if (p_m == p_m_pre)
        {
            p_m->next = p_n_next;
            return p_n;
        }
        else
        {
            p_m_pre->next = p_n;
            p_m->next = p_n_next;
            return head;
        }
    }
};


```