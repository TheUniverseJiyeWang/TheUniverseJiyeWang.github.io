---
layout:     post
title:      Solutions to Tree and Graph Problems in LeetCode(4)
subtitle:   LeetCode Problem No.116, No.230 and No.200
date:       2020-04-05
author:     JYW
header-img: img/post-200405-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.116 Populating Next Right Pointers in Each Node

#### Problem Statement

You are given a **perfect binary tree** where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:
```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

-**Follow up**：

1. You may only use constant extra space.
2. Recursive approach is fine, you may assume implicit stack space does not count as extra space for this problem.

-**Example**：

![sample](https://assets.leetcode.com/uploads/2019/02/14/116_sample.png)
```
Input: root = [1,2,3,4,5,6,7]
Output: [1,#,2,3,#,4,5,6,7,#]
Explanation: Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
```

-**Constraints**：

1. The number of nodes in the given tree is less than 4096.
2. -1000 <= node.val <= 1000

#### Solution

If we ignore the constraint of constant space, we can easily solve the problem by level order traverse.
```
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        level = [root]
        
        while level != []:
            queue = []
            if level[0] != None:
                for i in range(1,len(level)):
                    level[i-1].next = level[i]
            for node in level:
                if node != None:
                    queue.append(node.left)
                    queue.append(node.right)
            level = queue
        
        return root
``` 
Or we can use stack and `pop()`.
```
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        if not root:
            return None
        queue = [root]
        while queue:
            tmp = []
            for i in range(len(queue)):
                cur_node = queue.pop(0)
                tmp.append(cur_node)
                if cur_node.left:
                    queue.append(cur_node.left)
                if cur_node.right:
                    queue.append(cur_node.right)
            for i in range(len(tmp) - 1):
                tmp[i].next = tmp[i + 1]
        return root
```

# Problem No.230 Kth Smallest Element in a BST

#### Problem Statement

Given a binary search tree, write a function `kthSmallest` to find the **k**th smallest element in it.

-**Note**:

You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

-**Example**：

```
Input: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
Output: 1

Input: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
Output: 3
```

-**Follow up**:

What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

#### Solution

We can definitely traverse the whole binary tree and sort the value. But it's not the best solution.
```
class Solution:
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        nodes = []
        node = root
        res = []
        while nodes != [] or node != None:
            if node != None:
                nodes.append(node)
                node = node.left
            else:
                tmp = nodes.pop()
                res.append(tmp.val)
                node = tmp.right
        res.sort()
        return res[k-1]
``` 
However, since the numbers are sorted in inorder traverse, we can directly find the **k**th smallest number.
```
class Solution:
    def kthSmallest(self, root, k):
        stack = []
        
        while True:
            while root:
                stack.append(root)
                root = root.left
            root = stack.pop()
            k -= 1
            if not k:
                return root.val
            root = root.right
```

# Problem No.200 Number of Islands

#### Problem Statement

Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

-**Example**：
```
Input:
11110
11010
11000
00000

Output: 1

Input:
11000
11000
00100
00011

Output: 3
```

#### Solution

We can traverse the 2d grid and each time we find a `1`, we can search for all the `1`s connected to it and add them into a `set()`. Then we can just look for all the `1`s which is not in the set.
```
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        s = []
        visited = set()
        
        neighbors = [         (0, 1),
                     (-1, 0),         (1, 0),
                              (0,-1),]
        
        count = 0
        
        for i in range(len(grid)):
            for j in range(len(grid[i])):
                if ((i, j) not in visited) and grid[i][j] == '1':
                
                    s.append((i, j))
                    visited.add((i, j))
                    count += 1
                    
                    while s:
                        node = s.pop()
                        x, y = node
                        
                        for p, q in neighbors:
                            new_x = x + p
                            new_y = y + q
                            
                            if new_x in range(len(grid)) and new_y in range(len(grid[0])):
                                if (new_x, new_y) not in visited and grid[new_x][new_y] == '1':
                                    s.append((new_x, new_y))
                                    visited.add((new_x, new_y))
                                    
        return count
```
