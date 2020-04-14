---
layout:     post
title:      Solutions to Other Problems in LeetCode(4)
subtitle:   LeetCode Problem No.621 and No.406
date:       2020-04-11
author:     JYW
header-img: img/post-200411-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.621 Task Scheduler

#### Problem Statement

Given a char array representing tasks CPU need to do. It contains capital letters A to Z where different letters represent different tasks. Tasks could be done without original order. Each task could be done in one interval. For each interval, CPU could finish one task or just be idle.

However, there is a non-negative cooling interval n that means between two **same tasks**, there must be at least n intervals that CPU are doing different tasks or just be idle.

You need to return the **least** number of intervals the CPU will take to finish all the given tasks.

-**Example**：
```
Input: tasks = ["A","A","A","B","B","B"], n = 2
Output: 8
Explanation: A -> B -> idle -> A -> B -> idle -> A -> B.
```

-**Constraints**:

1. The number of tasks is in the range `[1, 10000]`.
2. The integer `n` is in the range `[0, 100]`.

#### Solution

The tricky part of the problem is to count how many idles are there and we can add the number of idles to the length of tasks.
```
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        tasks_count = [0 for i in range(26)]
        res = len(tasks)
        for task in tasks:
            tasks_count[ord(task)-65] += 1
        tasks_count.sort(reverse = True)
        i = 0
        while i < len(tasks_count):
            if tasks_count[i] == 0:
                tasks_count[:] = tasks_count[:i]
                break
            i += 1
        l = len(tasks_count)
        print(tasks_count)
        maxtask = tasks_count[0]
        idle = (maxtask-1)*n
        idle -= sum(tasks_count[1:])
        i = 1
        while i < l and tasks_count[i] == tasks_count[0]:
            idle += 1
            i += 1
        if idle < 0:
            idle = 0
        res += idle
        return res
``` 

# Problem No.406 Queue Reconstruction by Height

#### Problem Statement

Suppose you have a random list of people standing in a queue. Each person is described by a pair of integers `(h, k)`, where h is the height of the person and k is the number of people in front of this person who have a height greater than or equal to h. Write an algorithm to reconstruct the queue.

-**Note**:

The number of people is less than 1,100.

-**Example**
```
Input:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

Output:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
```

#### Solution

The solution is to sort the list first by height and then by the number of people in front with a higher or equal height. And we can assign a empty list each people by people. This method is with a `O(n**2)` time complexity.
```
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        if len(people) == 1:
            return people
        res = [[]] * (len(people))        
        for i in range(0, len(people)):
            people[i] = people[i][0]+people[i][1]/10000
        people.sort()
        for i in range(0, len(people)):
            h_ppl = int(people[i])
            k_ppl = round((people[i]-int(people[i]))*10000)
            k_ind = 0
            for k in range(0, len(people)-1):
                if k_ppl == 0:  # If k_ppl ==0, find the first none to assign the current element
                    while res[k]:
                        k += 1
                    res[k] = [h_ppl, k_ppl]
                    break
                if res[k] == []:
                    k_ind += 1
                else:
                    if res[k][0] == h_ppl:
                        k_ind += 1
                if (k_ind == k_ppl) and (not res[k+1]):
                    res[k+1] = [h_ppl, k_ppl]
                    break
        return res
``` 

However, if we apply binary search tree, the time complexity can be reduced to `O(n log n)`.
```

class TreeNodeWithSize:
    def __init__(self,val,size):
        self.val=val
        self.size=size
        self.removed=False
        self.left=None
        self.right=None

class IndsBST:
    def __init__(self,n):
        self.root=self.constructBST(0,n)

    def constructBST(self,start,end):
        if start>=end:
            return None
            
        mid=(start+end)//2
        root=TreeNodeWithSize(mid,end-start)
        root.left=self.constructBST(start,mid)
        root.right=self.constructBST(mid+1,end)
        return root
    
    def getKthNodeIndex(self,k):
        return self._getKthNodeIndex(k,self.root)
        
    def _getKthNodeIndex(self,k,root):
        
        leftSize= root.left.size if root.left else 0
        root.size-=1 #reduce size of this node
        if k==leftSize and not root.removed: #found node
            root.removed=True
            return root.val
        if k>=leftSize: #go right
            return self._getKthNodeIndex(k-leftSize-int(not root.removed),root.right)
        else:
            return self._getKthNodeIndex(k,root.left)
        
class Solution:
    
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        
        people=sorted(people,key=lambda person: person[1],reverse=True)
        people=sorted(people,key=lambda person: person[0])
        
        out=[None]*len(people)
        indsTree=IndsBST(len(people))
        
        for person in people:
            out[indsTree.getKthNodeIndex(person[1])]=person
        return out
```

