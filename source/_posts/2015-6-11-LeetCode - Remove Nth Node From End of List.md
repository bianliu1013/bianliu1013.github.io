---
layout: post
title: leetcode -  Remove Nth Node From End of List!
date:   2015-06-11 11:23:32
categories: algothrim
tags: algothrim
---


Given a linked list, remove the nth node from the end of list and return its head.


For example,

---

   Given linked list: 1->2->3->4->5, and n = 2.

   After removing the second node from the end, the linked list becomes 1->2->3->5.

---

Note:

Given n will always be valid.

Try to do this in one pass. 


### code:

``` c


class Solution {
  public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if (head == NULL) {
            return NULL;
        } else if (0 == n) {
            return head;
        }

        ListNode* p_now = head;
        ListNode* p_found = head;
        ListNode* p_found_pre = head;

        int step = 0;
        int found_step = step - n;
        while (p_now->next != NULL) {
            p_now = p_now->next;
            found_step++;
            if (found_step >= 0) {
                p_found = p_found->next;
                if ((found_step - 1) >= 0) {
                    p_found_pre = p_found_pre->next;
                }
            }
        }

        if (head == p_found) {
            return p_found->next;
        } else {
            p_found_pre->next = p_found->next;
        }
        return head;
    }
};

```