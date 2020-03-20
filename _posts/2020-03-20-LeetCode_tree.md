---
layout:     post
title:      Solutions to Tree Problems in LeetCode(2)
subtitle:   LeetCode Problem No.101, No.107 and No.108
date:       2020-03-20
author:     JYW
header-img: img/post-200320-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.104 Symmetric Tree

#### Problem Statement

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

-**Example**：

For example, this binary tree `[1,2,2,3,4,4,3]` is symmetric:
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
But the following `[1,2,2,null,3,null,3]` is not:
```
    1
   / \
  2   2
   \   \
   3    3
```

#### Solution

At first, I used Level Traverse but the runtime is extremely high.
```
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        if root == None:
            return True
        if root.left == None and root.right == None:
            return True
        elif root.left != None and root.right != None and root.left.val == root.right.val:
            level = [root.left, root.right]
        else:
            return False
        while len(set(level)) != 1:
            queue = []
            queue_val = []
            for el in level:
                if el == "None":
                    queue.append("None")
                    queue.append("None")
                else:
                    if el.left:
                        queue.append(el.left)
                    else:
                        queue.append("None")
                    if el.right:
                        queue.append(el.right)
                    else:
                        queue.append("None")
            for each in queue:
                if each == "None":
                    queue_val.append("None")
                else:
                    queue_val.append(each.val)
            queue_val_rv = queue_val.copy()
            queue_val_rv.reverse()
            if queue_val != queue_val_rv:
                return False
            level = queue
        return True
``` 
Then I realized why can't I use two Inorder Tranverse at the same time, one from left and one from right. The runtime reduced from 1596ms to 32ms.
```
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        if root == None:
            return True
        nodes_left = []
        nodes_right = []
        node_left = root
        node_right = root
        while node_left != None or node_right != None or nodes_left != [] or nodes_right != []:
            if node_left == root.right or node_right == root.left:
                break
            if node_left != None and node_right != None:
                if node_left.val == node_right.val:
                    nodes_left.append(node_left)
                    node_left = node_left.left
                    nodes_right.append(node_right)
                    node_right = node_right.right
                else:
                    return False
            elif node_left == None and node_right == None:
                node_left = nodes_left.pop()
                node_right = nodes_right.pop()
                node_left = node_left.right
                node_right = node_right.left
            else:
                return False
        return True
```
Or we can do it recursively, of course.
```
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        
        def isMirror(t1: TreeNode, t2: TreeNode) -> bool:
            if t1 == None and t2 == None:
                return True
            if t1 == None or t2 == None:
                return False
            if t1.val == t2.val and isMirror(t1.right,t2.left) == True and isMirror(t1.left, t2.right) == True:
                return True
            else:
                return False
        
        return isMirror(root, root)
```

# Problem No.107 Binary Tree Level Order Traversal II

#### Problem Statement

Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

-**Example**：

Given binary tree `[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```
return its bottom-up level order traversal as:
```
[
  [15,7],
  [9,20],
  [3]
]
```

#### Solution

I just need to implement Level Traverse and record the values of ndoes in each level, then reverse the result list.
```
class Solution:
    def levelOrderBottom(self, root: TreeNode) -> List[List[int]]:
        if root == None:
            return []
        result = [[root.val]]
        level = [root]
        while level:
            queue = []
            for el in level:
                if el.left:
                    queue.append(el.left)
                if el.right:
                    queue.append(el.right)
            tmp = []
            for each in queue:
                if each != None:
                    tmp.append(each.val)
            result.append(tmp)
            level = queue
        result.pop()
        result.reverse()
        return result
``` 
Or we can do it in a DFS way.
```
class Solution:
    def levelOrderBottom(self, root: TreeNode) -> List[List[int]]:
        stack = [(root, 0)]
        res = []
        while stack:
            node, level = stack.pop()
            if node:
                if len(res) < level+1:
                    res.insert(0, [])
                res[-(level+1)].append(node.val)
                stack.append((node.right, level+1))
                stack.append((node.left, level+1))
        return res
```

# Problem No.108 Convert Sorted Array to Binary Search Tree

#### Problem Statement

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

-**Example**：

```
Given the sorted array: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
```

#### Solution

My first thought was to reshaping the `nums` list to a suitable and using Level Traverseform to create the height balanced BST. However, when I implemented this solution, I found the time complexity could be really high and extra space is needed to store the reshaped list. So I changed the idea and implemented recursive solution.
```
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
        if not nums:
            return None

        mid = len(nums) // 2

        root = TreeNode(nums[mid])
        root.left = self.sortedArrayToBST(nums[:mid])
        root.right = self.sortedArrayToBST(nums[mid+1:])

        return root
``` 
