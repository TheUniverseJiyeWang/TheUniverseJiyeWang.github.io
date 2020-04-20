---
layout:     post
title:      Solutions to Linked List Problems in LeetCode(5)
subtitle:   LeetCode Problem No.23, No.148 and No.138
date:       2020-04-14
author:     JYW
header-img: img/post-200414-LeetCode1.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.23 Merge k Sorted Lists

#### Problem Statement

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

-**Example**
```
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
```

#### Solution

We can always use extra space to store all values of all nodes. Then we just need to sort the values and create a new linked list. This method is with `O(n log n)` time complexity. But the space complexity is quite large.
```
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        res = []
        for node in lists:
            while node != None:
                res.append(node.val)
                node = node.next
        res.sort()
        if res == []:
            return None
        head = ListNode(res[0])
        node = head
        for i in range(1, len(res)):
            node.next = ListNode(res[i])
            node = node.next
        return head
```
Or we can use priority queue with the space complexity `O(k)` and time complexity `O(n log k)`.
```
from Queue import PriorityQueue

class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        head = point = ListNode(0)
        q = PriorityQueue()
        for l in lists:
            if l:
                q.put((l.val, l))
        while not q.empty():
            val, node = q.get()
            point.next = ListNode(val)
            point = point.next
            node = node.next
            if node:
                q.put((node.val, node))
        return head.next
``` 

We also have done a question about merging two sorted linked list. So, here we can merge two linked lists at one time until all the lists are merged into the final result.
```
class Solution(object):
    def mergeKLists(self, lists):
        if lists == []:
            return None
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        amount = len(lists)
        interval = 1
        while interval < amount:
            for i in range(0, amount - interval, interval * 2):
                lists[i] = self.merge2Lists(lists[i], lists[i + interval])
            interval *= 2
        return lists[0] if amount > 0 else lists

    def merge2Lists(self, l1, l2):
        head = point = ListNode(0)
        while l1 and l2:
            if l1.val <= l2.val:
                point.next = l1
                l1 = l1.next
            else:
                point.next = l2
                l2 = l1
                l1 = point.next.next
            point = point.next
        if not l1:
            point.next=l2
        else:
            point.next=l1
        return head.next
```
# Problem No.148 Sort List

#### Problem Statement

Sort a linked list in `O(n log n)` time using constant space complexity.

-**Example**:

```
Input: 4->2->1->3
Output: 1->2->3->4

Input: -1->5->3->4->0
Output: -1->0->3->4->5
```

#### Solution

If we sort it by brute force, the time complexity will be `O(n**2)`. Or we can also use extra space to store all the values, sort the list and create a new linked list.
```
class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        if head == None:
            return None
        node = head
        tail = head
        while node != None:
            tmp = node
            node = node.next
            if tmp.val <= head.val:
                tmp.next = head
                head = tmp
            elif tmp.val >= tail.val:
                tail.next = tmp
                tail = tmp
            else:
                pos = head
                while pos.next != None:
                    if pos.val <= tmp.val <= pos.next.val:
                        pos_next = pos.next
                        pos.next = tmp
                        tmp.next = pos_next
                        break
                    pos = pos.next
            
        tail.next = None
        return head
``` 
Or we can apply quick sort to linked list.
```
class Solution:
    def sortList(self, head: ListNode) -> ListNode:  
        def quicksort(head, tail):
            if head == tail:
                return head          
            def partition(head, tail): 
                temp = ListNode(0)
                temp.next = head 
                pivot = head
                prev = head
                cur = head.next
                while cur != tail: 
                    if cur.val < pivot.val:
                        next = cur.next
                        prev.next = cur.next
                        cur.next = temp.next
                        temp.next = cur
                        cur = next
                    elif cur.val > pivot.val:
                        prev = cur
                        cur = cur.next
                    else:
                        next = cur.next
                        prev.next = cur.next
                        cur.next = pivot.next
                        pivot.next = cur
                        cur = next
                        if prev==pivot:
                            prev = prev.next
                        pivot = pivot.next   
                return temp.next, pivot
            left, pivot = partition(head, tail)
            left = quicksort(left, head)
            pivot.next = quicksort(pivot.next, tail)
            return left
        
        return quicksort(head, None)
```

# Problem No.138 Copy List with Random Pointer

#### Problem Statement

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a **deep copy** of the list.

The Linked List is represented in the input/output as a list of *n* nodes. Each node is represented as a pair of `[val, random_index]` where:

`val`: an integer representing `Node.val`
`random_index`: the index of the node (range from `0` to `n-1`) where random pointer points to, or `null` if it does not point to any node.

-**Example**：

![Example 1](https://assets.leetcode.com/uploads/2019/12/18/e1.png)
```
Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]
```
![Example 2](https://assets.leetcode.com/uploads/2019/12/18/e2.png)
```
Input: head = [[1,1],[2,1]]
Output: [[1,1],[2,1]]
```
![Example 3](https://assets.leetcode.com/uploads/2019/12/18/e3.png)
```
Input: head = [[3,null],[3,0],[3,null]]
Output: [[3,null],[3,0],[3,null]]

Input: head = []
Output: []
Explanation: Given linked list is empty (null pointer), so return null.
```

-**Constraints**:

1. `-10000 <= Node.val <= 10000`
2. `Node.random` is null or pointing to a node in the linked list.
3. Number of Nodes will not exceed 1000.

#### Solution

We can always solve it by brute force, creating a new linked list and repeat everything when we traverse the original linked list. The good thing of the solution is that it doesn't need extra space.
```
class Solution:
    def copyRandomList(self, head: 'Node') -> 'Node':
        if not head:
            return None
        res_head = Node(head.val)
        tmp_res = res_head
        tmp = head.next
        while tmp != None:
            tmp_res.next = Node(tmp.val)
            tmp = tmp.next
            tmp_res = tmp_res.next
        tmp_res = res_head
        tmp = head
        while tmp != None:
            if tmp.random != None:
                pos = head
                c = 0
                while pos != tmp.random:
                    pos = pos.next
                    c += 1
                p = res_head
                i = 0
                while i < c:
                    p = p.next
                    i += 1
                tmp_res.random = p
            tmp = tmp.next
            tmp_res = tmp_res.next
        return res_head
```

Or we can always use set().
```
class Solution:
    def copyRandomList(self, head: 'Node') -> 'Node':
        seen = {}
        def dfs(head):
            if head in seen:
                return seen[head]
            if not head:
                return None
            cp = Node(head.val)
            seen[head] = cp
            cp.next = dfs(head.next)
            cp.random = dfs(head.random)
            return cp
        return dfs(head)
```