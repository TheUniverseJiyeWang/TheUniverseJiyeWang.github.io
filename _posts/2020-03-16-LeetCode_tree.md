---
layout:     post
title:      Solutions to Tree Problems in LeetCode(1)
subtitle:   LeetCode Problem No.104 and No.98
date:       2020-03-16
author:     JYW
header-img: img/post-200316-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.104 Maximum Depth of Binary Tree

#### Problem Statement

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

-**Note**:

A leaf is a node with no children.

-**Example**：

Given binary tree `[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```
return its depth = 3.

#### Solution

My first solution is to traverse the binary tree and store all nodes as indexes and depths as values into dataType `dict`. Then I just need to find the max one. This method is DFS.
```
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        nodes = [root]
        nodes_depth = {root:1}
        if root == None:
            return 0
        while nodes != []:
            tmp_node = nodes[-1]
            nodes.remove(tmp_node)
            tmp_depth = nodes_depth[tmp_node]
            if tmp_node.left != None:
                nodes.append(tmp_node.left)
                nodes_depth[tmp_node.left] = tmp_depth + 1
            if tmp_node.right != None:
                nodes.append(tmp_node.right)
                nodes_depth[tmp_node.right] = tmp_depth + 1
        return max(nodes_depth.values())
``` 
Or we can do it recursively.
```
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if root == None: return 0
        return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1
```
Or we can do it level by level, aka, BFS.
```
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        depth = 0
        level = [root] if root else []
        while level:
            depth += 1
            queue = []
            for el in level:
                if el.left:
                    queue.append(el.left)
                if el.right:
                    queue.append(el.right)
            level = queue
        return depth
```
Another BFS.
```
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        from collections import deque
        queue = deque([(root, 1)])
        while queue:
            curr, val = queue.popleft()
            if not curr.left and not curr.right and not queue:
                return val
            if curr.left:
                queue.append((curr.left, val+1))
            if curr.right:
                queue.append((curr.right, val+1))
```
# Problem No.98 Validate Binary Search Tree

#### Problem Statement

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:
1. The left subtree of a node contains only nodes with keys **less than** the node's key.
2. The right subtree of a node contains only nodes with keys **greater than** the node's key.
3. Both the left and right subtrees must also be binary search trees.

-**Example**
```
    2
   / \
  1   3

Input: [2,1,3]
Output: true

    5
   / \
  1   4
     / \
    3   6

Input: [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

#### Solution

My solution is to right a function to inorder traverse the Tree, aka, from left leave to root and to right leave. Then I just need to check if all the elements is sorted and different.
```
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        """
        :type root: TreeNode
        :rtype: bool
        """
        if root == None:
            return True
        min1 = -float('inf')
        nodes = []
        node = root
        while node != None or nodes != []:
            if node != None:
                nodes.append(node)
                node = node.left
            else:
                tmp = nodes[-1]
                nodes.remove(tmp)
                if tmp.val > min1:
                    min1 = tmp.val
                else:
                    return False
                node = tmp.right
        return True
``` 
Or we can do it recursively.
```
class Solution:
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        def helper(node, lower = float('-inf'), upper = float('inf')):
            if not node:
                return True
            
            val = node.val
            if val <= lower or val >= upper:
                return False

            if not helper(node.right, val, upper):
                return False
            if not helper(node.left, lower, val):
                return False
            return True

        return helper(root)
```
Or iteratively.
```
class Solution:
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if not root:
            return True
            
        stack = [(root, float('-inf'), float('inf')), ] 
        while stack:
            root, lower, upper = stack.pop()
            if not root:
                continue
            val = root.val
            if val <= lower or val >= upper:
                return False
            stack.append((root.right, val, upper))
            stack.append((root.left, lower, val))
        return True  
```
