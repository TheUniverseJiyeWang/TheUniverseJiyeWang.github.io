---
layout:     post
title:      Solutions to Linked List Problems in LeetCode(1)
subtitle:   LeetCode Problem No.83 and No.02
date:       2020-03-04
author:     JYW
header-img: img/post-200304-LeetCode1.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.83 Remove Duplicates from Sorted List

#### Problem Statement

Given a sorted linked list, delete all duplicates such that each element appear only once.

-**Example**
```
Input: 1->1->2
Output: 1->2
Input: 1->1->2->3->3
Output: 1->2->3
```

#### Solution

The tricky part of this solution is shrinking the linked list by removing duplicate nodes in the list by using a third node as a relay station.
```
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if head == None:
            return head
        current = head
        while current != None and current.next != None:
            if current == current.next:
                tmp_next = current.next
                current.next = tmp_next.next
        return head
``` 

# Problem No.02 Add Two Numbers

#### Problem Statement

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

-**Example**
```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

#### Solution

The tricky part of this solution is the carry-over. Besides, I have to consider multiple edging conditions such as the last adding create an additional digit, or two linked lists are not of the same length.
```
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        l1_tmp = l1.val
        l2_tmp = l2.val
        head = (l1_tmp + l2_tmp)%10
        result = ListNode(head)  
        result_head = result
        sum_tmp = l1_tmp + l2_tmp
        more_tmp = sum_tmp//10
        while l1.next != None or l2.next != None:       
            if l1.next == None:
                l1.next = ListNode(0)
                l1.next.next = None
            if l2.next == None:
                l2.next = ListNode(0)
                l2.next.next = None
            l1 = l1.next
            l2 = l2.next
            l1_tmp = l1.val
            l2_tmp = l2.val
            left_tmp = (l1_tmp + l2_tmp + more_tmp)%10
            more_tmp = (l1_tmp + l2_tmp + more_tmp)//10
            result.next = ListNode(left_tmp)
            result = result.next
        else:
            if more_tmp > 0:
                result.next = ListNode(more_tmp)
                result = result.next
                result.next = None
            else:
                result.next = None
        return result_head
``` 