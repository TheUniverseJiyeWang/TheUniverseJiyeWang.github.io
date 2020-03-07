---
layout:     post
title:      Solutions to Linked List Problems in LeetCode(2)
subtitle:   LeetCode Problem #237 and #19
date:       2020-03-07
author:     JYW
header-img: img/post-200307-LeetCode1.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.237 Delete Node in a Linked List

#### Problem Statement

Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

Given linked list -- head = [4,5,1,9], which looks like following:
```
4->5->1->9
```
-**Example**
```
Input: head = [4,5,1,9], node = 5
Output: [4,1,9]
Explanation: You are given the second node with value 5, the linked list should become 4 -> 1 -> 9 after calling your function.
```

#### Solution

The question and its answer is really simple and stupid, as shown below. 

```
class Solution:
    def deleteNode(self, node):
        node.val = node.next.val
        node.next = node.next.next
```
However, the **standard solution** does not fit the problem description. So I write another function that fits it where the input is a list `head` and a value of the `node` needed to be deleted. 
```
class Solution:
    def deleteNode(head: [int], node: int) -> ListNode:
        start = ListNode(head[0])
        current = start
        for i in range(1,len(head)):
            current.next = ListNode(head[i])
            current = current.next
        current.next = None
        current = start
        if current.val == node:
            current.val = current.next.val
            current.next = current.next.next
            return start
        while current != None and current.next != None:
            if current.next.val == node:
                if current.next.next != None:
                    current.next.val = current.next.next.val
                    current.next = current.next.next
                else: 
                    current.next = None
        return start
``` 
I know it can also be simply solved in a way shown below. But in that case, it doesn't include any deletion in linked list.
```
class Solution:
    def deleteNode(head: [int], node: int) -> ListNode:
        start = ListNode(head[0])
        current = start
        for i in range(1,len(head)):
            if head[i] != node:
                current.next = ListNode(head[i])
                current = current.next
        return start
```

# Problem No.19 Remove Nth Node From End of List

#### Problem Statement

Given a linked list, remove the n-th node from the end of list and return its head.

**Note**: Given n will always be valid.
**Follow up**: Could you do this in one pass?

-**Example**
```
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```

#### Solution

I used two pointers to solve the problem. The first pointer starts from the head and the second pointer starts from the **n**-th elements of the linked list. When they moved forwards at the same time, the second pointer will reach the `None` value, aka the end of the linked likst. At this time, the first pointer points at the **n**-th node from the end of the linked list. Then I just need to delete the node.

**Note**: When writing the function, there are multiple extreme condition needed to be taken into consideration. For example, the first node is the one needed to be removed, or there is only one node in the linked list. Besides, I used "**current.next = current.next.next**" to delete the node. Thus, there is one more condition that n is a small number and `current.next.next` is `None`.
```
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        current = head
        current_n = current
        if current.next == None:
            return None
        if n == 1:
            while current.next.next != None:
                current = current.next
            else:
                current.next = None
            return head
        for i in range(0, n-1):
            current_n = current_n.next
        if current_n.next == None:
            if current == head:
                return head.next
            else:
                current.next = current.next.next
                return head
        while current_n.next.next != None:
            current = current.next
            current_n = current_n.next
        else:
            current.next = current.next.next
        return head
``` 
Also, there is another simplified way to solve the problem using the same concept. The difference is if the n is very large, the runtime of this solution may perform better than the previous one.
```
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        current = head
        current_n = current
        distance = 1
        while current_n.next != None:
            distance += 1
            current_n = current_n.next
            if distance > n+1:
                current = current.next
        if distance == n:
            return head.next
        else:
            current.next = current.next.next
        return head
```