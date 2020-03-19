---
layout:     post
title:      Solutions to Linked List Problems in LeetCode(4)
subtitle:   LeetCode Problem No.141, No.328 and No.160
date:       2020-03-18
author:     JYW
header-img: img/post-200318-LeetCode1.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.141 Linked List Cycle

#### Problem Statement

Given a linked list, determine if it has a cycle in it.

To represent a cycle in the given linked list, we use an integer `pos` which represents the position (0-indexed) in the linked list where tail connects to. If `pos` is `-1`, then there is no cycle in the linked list.

-**Example**
```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the second node.

Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the first node.

Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```
-**Follow up**
Can you solve it using `O(1)` (i.e. constant) memory?

#### Solution

There is no input as pos in this problem and actually, we just need to check if the last node is linked to any previous node which create a cycle. However, we can't find `None` if the linked list is indeed a cycle. So the tricky part is how do we examine the same node and when do we break. By using two pointers, we can increase the length of cycle one node by one node and check if the same nodes meet.
```
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        if head == None:
            return
        fast_pointer = head.next
        slow_pointer = head
        while slow_pointer != fast_pointer:
            if fast_pointer == None or fast_pointer.next == None:
                return False
            slow_pointer = slow_pointer.next
            fast_pointer = fast_pointer.next.next
        else:
            return True
```
Or we can use `try-except` to achieve it.
```
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        try:
            slow = head
            fast = head.next
            while slow is not fast:
                slow = slow.next
                fast = fast.next.next
            return True
        except:
            return False
``` 

# Problem No.328 Odd Even Linked List

#### Problem Statement

Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in `O(1)` space complexity and `O(nodes)` time complexity.

-**Example**:

```
Input: 1->2->3->4->5->NULL
Output: 1->3->5->2->4->NULL

Input: 2->1->3->5->6->4->7->NULL
Output: 2->3->6->7->1->5->4->NULL
```
-**Note**:

The relative order inside both the even and odd groups should remain as it was in the input.
The first node is considered odd, the second node even and so on ...

#### Solution

This problem can be also solved by two pointers.
```
class Solution:
    def oddEvenList(self, head: ListNode) -> ListNode:
        if head == None or head.next == None:
            return head
        odd = head
        even = head.next
        even_head = head.next
        while even.next != None and even.next.next != None:
            odd.next = even.next
            even.next = even.next.next
            odd.next.next = even_head
            odd = odd.next
            even = even.next
        else:
            if even.next != None:
                odd.next = even.next
                even.next = None
                odd.next.next = even_head
        return head
``` 
Or we can do it more clear
```
class Solution:
    def oddEvenList(self, head: ListNode) -> ListNode:
        if not head:
            return head
        odd=head
        even=head.next
        while even and even.next!=None:
            temp = even.next
            even.next = even.next.next
            temp.next = odd.next
            odd.next = temp
            odd=odd.next
            even=even.next
        return head
```
If we ignore the requirement of `O(1)`  space, the run time could be faster.
```
class Solution:
    def oddEvenList(self, head: ListNode) -> ListNode:
        d1=odd=ListNode(0)
        d2=even=ListNode(0)
        i=1
        while head:
            if i%2:
                odd.next,odd=head,head
            else:
                even.next,even=head,head
            head=head.next
            i+=1
        odd.next,even.next=d2.next,None
        return d1.next
```

# Problem No.160 Intersection of Two Linked Lists

#### Problem Statement

Write a program to find the node at which the intersection of two singly linked lists begins.

For example, the following two linked lists:
![begin to intersect at node c1.](https://assets.leetcode.com/uploads/2018/12/13/160_statement.png)

-**Example**：

![Example 1](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)
```
Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
Output: Reference of the node with value = 8
Input Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,0,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
```
![Example 2](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)
```
Input: intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
Output: Reference of the node with value = 2
Input Explanation: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [0,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.
```
![Example 3](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)
```
Input: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
Output: null
Input Explanation: From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values.
Explanation: The two lists do not intersect, so return null.
```

-**Notes**:

If the two linked lists have no intersection at all, return `null`.
The linked lists must retain their original structure after the function returns.
You may assume there are no cycles anywhere in the entire linked structure.
Your code should preferably run in `O(n)` time and use only `O(1)` memory.

#### Solution

The first thought is to use two pointers to slightly increase the distance between two linked liksts until they intersect. However, due to different conditions that this methods won't work or meet the requirement of `O(n)` time complexity. So I decide to first calculate the lengths respectively and then use the fixed distance to find the intersected node.
```
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        lengthA = 1
        lengthB = 1
        nodeA = headA
        nodeB = headB
        while nodeA != None:
            nodeA = nodeA.next
            lengthA +=1
        while nodeB != None:
            nodeB = nodeB.next
            lengthB +=1
        distance = abs(lengthA - lengthB)
        if lengthA >= lengthB:
            slow = headB
            fast = headA
        else:
            slow = headA
            fast = headB
        for i in range(0, distance):
            fast = fast.next
        while slow != None and fast != None:
            if slow == fast:
                return slow
            slow = slow.next
            fast = fast.next
        return None
```

Or we can direct the nodes to the head of another linked lists when it reached the end of the original linked list which creates a same distance to traverse two linked lists for at most nearly two times and check if there is a same node.
```
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        if headA == None or headB == None:
            return None

        A_pointer = headA
        B_pointer = headB

        while A_pointer != B_pointer:
            A_pointer = headB if A_pointer == None else A_pointer.next
            B_pointer = headA if B_pointer == None else B_pointer.next

        return A_pointer
```