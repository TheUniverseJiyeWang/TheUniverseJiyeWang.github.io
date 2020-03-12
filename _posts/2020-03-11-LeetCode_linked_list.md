---
layout:     post
title:      Solutions to Linked List Problems in LeetCode(3)
subtitle:   LeetCode Problem #206, #21 and #234
date:       2020-03-11
author:     JYW
header-img: img/post-200311-LeetCode1.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.206 Reverse Linked List

#### Problem Statement

Reverse a singly linked list.

-**Example**
```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```
-**Follow up**
A linked list can be reversed either iteratively or recursively. Could you implement both?

#### Solution

It's a bit more complex than just delete one node. But using two extra space, it can be easily done. This solution is iterative.
```
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        prev = None
        current = head
        while current != None:
            next_tmp = current.next
            current.next = prev
            prev = current
            current = next_tmp
        return prev
```
Or we can do it recursively with more space used. This solution is a bit hard to think through.
```
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if head == None or head.next == None:
            return head
        prev = self.reverseList(head.next)
        head.next.next = head
        head.next = None
        return prev
``` 
For better understanding, we can run this one and see what's printed in the console. The recursive solution will always run from the bottom layer which return the **last element** in the linked list.
```
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None
        
        
head = ListNode(1)
current = head
for i in range(2,6):
    current.next = ListNode(i)
    current = current.next
    
class solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if head == None or head.next == None:
            return head
        prev = self.reverseList(head.next)
        head.next.next = head
        head.next = None
        print(str("prev: ")+str(prev.val)+str(" head: ")+str(head.val)+str("->")+str(head.next))
        return prev

rv = solution()

reverse_head = rv.reverseList(head)
```

# Problem No.21 Merge Two Sorted Lists

#### Problem Statement

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

-**Example**
```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

#### Solution

This problem can be also solved by two pointers.
```
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if l1 == None and l2 == None:
            return None
        if l1 == None:
            return l2
        if l2 == None:
            return l1
        if l1.val <= l2.val:
            head = l1
            cur_l1 = l1.next
            cur_l2 = l2
        else:
            head = l2
            cur_l1 = l1
            cur_l2 = l2.next
        current = head
        while cur_l1 != None and cur_l2 != None:
            if cur_l1.val <= cur_l2.val:
                current.next = cur_l1
                cur_l1 = cur_l1.next
            else:
                current.next = cur_l2
                cur_l2 = cur_l2.next
            current = current.next
        while cur_l1 != None:
            current.next = cur_l1
            cur_l1 = cur_l1.next
            current = current.next
        while cur_l2 != None:
            current.next = cur_l2
            cur_l2 = cur_l2.next
            current = current.next
        return head
``` 

# Problem No.234 Palindrome Linked List

#### Problem Statement

Given a singly linked list, determine if it is a palindrome.

-**Example**
```
Input: 1->2
Output: false
Input: 1->2->2->1
Output: true
```
-**Follower up**
Could you do it in O(n) time and O(1) space?

#### Solution

The first thought is still two pointers. I used the concept of iterative reversed linked list and compared with the rest part of linked list. To be more specific, two dataType `boolean` to check if it's a palindrome in the current length and if it runs through the whole linked list.
```
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        if head == None or head.next == None:
            return True
        if head.next.next == None and head.val == head.next.val:
            return True
        current = head
        rv_cur = head
        rv_prev = None
        ispalin = False
        isall = False
        while ispalin == False or isall == False:
            next_tmp = current.next
            current.next = rv_prev
            rv_prev = current
            current = next_tmp
            if current.next == None:
                break
            if rv_prev.val == current.next.val:
                eq1 = rv_prev
                eq2 = current.next
                ispalin = True
                while eq1.val == eq2.val:
                    eq1 = eq1.next
                    eq2 = eq2.next
                    if eq1 == None and eq2 == None:
                        isall = True
                        break
                    elif eq1 == None or eq2 == None:
                        ispalin = False
                        break
                else:
                    ispalin = False
            elif rv_prev.val == current.val:
                eq1 = rv_prev
                eq2 = current
                ispalin = True
                while eq1.val == eq2.val:
                    eq1 = eq1.next
                    eq2 = eq2.next
                    if eq1 == None and eq2 == None:
                        isall = True
                        break
                    elif eq1 == None or eq2 == None:
                        ispalin = False
                        break
                else:
                    ispalin = False
        return ispalin
```

Or we can first find the middle node and then reverse the second half and then compare.
```
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        fast = slow = head
        # find the mid node
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
        # reverse the second half
        node = None
        while slow:
            nxt = slow.next
            slow.next = node
            node = slow
            slow = nxt
        # compare the first and second half nodes
        while node: # while node and head:
            if node.val != head.val:
                return False
            node = node.next
            head = head.next
        return True
```