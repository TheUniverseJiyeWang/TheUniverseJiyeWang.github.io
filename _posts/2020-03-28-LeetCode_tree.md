---
layout:     post
title:      Solutions to Tree Problems in LeetCode(3)
subtitle:   LeetCode Problem No.94, No.103 and No.105
date:       2020-03-28
author:     JYW
header-img: img/post-200328-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.94 Binary Tree Inorder Traversal

#### Problem Statement

Given a binary tree, return the *inorder* traversal of its nodes' values.

-**Example**：

For example, this binary tree `[1,2,2,3,4,4,3]` is symmetric:
```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,3,2]
```

-**Follow up**：

Recursive solution is trivial, could you do it iteratively?


#### Solution

For now, it's easy to implement inorder traverse lol.
```
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        nodes = []
        node = root 
        while node != None or nodes != []:
            if node != None:
                nodes.append(node)
                node = node.left
            else:
                node = nodes.pop()
                if node != None:
                    res.append(node.val)
                node = node.right
        return res
``` 

# Problem No.103 Binary Tree Zigzag Level Order Traversal

#### Problem Statement

Given a binary tree, return the *zigzag level order* traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

-**Example**：

Given binary tree `[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```
return its zigzag level order traversal as:
```
[
  [3],
  [20,9],
  [15,7]
]
```

#### Solution

It's easy to implement *level order* traverse and add an indicator to check the odd or even depth.
```
class Solution:
    def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
        level = [root]
        res = []
        depth = 1
        while level != []:
            next_level = []
            tmp = []
            for each in level:
                if each != None:
                    tmp.append(each.val)
                    next_level.append(each.left)
                    next_level.append(each.right)
            if depth%2 == 0:
                tmp.reverse()
            res.append(tmp)
            depth += 1
            level = next_level
        return res[:-1]
``` 
Or append one element at one time.
```
class Solution:
    def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
        level = [root]
        res = []
        depth = 1
        while level != []:
            next_level = []
            res.append([])
            level.reverse()
            for each in level:
                if each != None:
                    res[depth-1].append(each.val)
                    if depth%2 == 1:
                        next_level.append(each.left)
                        next_level.append(each.right)
                    else:
                        next_level.append(each.right)
                        next_level.append(each.left)
            depth += 1
            level = next_level
        return res[:-1]
```
Or we can reverse the even level after appending all values.
```
class Solution:
    def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
        if not root:return []
        res=[]
        q=[root]
        
        while q:
            res.append([ x.val for x in q]) 
            q=[ c for x in q for c in (x.left,x.right) if c ]
            
        return [x[::-1] if i%2 else x for i,x in enumerate(res)]
```

# Problem No.105 Construct Binary Tree from Preorder and Inorder Traversal

#### Problem Statement

Given preorder and inorder traversal of a tree, construct the binary tree.

-**Note**:

You may assume that duplicates do not exist in the tree.

-**Example**：

For example, given
```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```
Return the following binary tree:
```
    3
   / \
  9  20
    /  \
   15   7
```

#### Solution

We can solve it recursively by finding the root node.
```
class Solution:
	def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
		if not preorder or not inorder:
			return None
		root = TreeNode(preorder[0])
		idx = inorder.index(preorder[0])
		root.left = self.buildTree(preorder[1:idx + 1], inorder[:idx])
		root.right = self.buildTree(preorder[idx + 1:], inorder[idx + 1:])
		return root
``` 
Or we can use map to optimize the solution.
```
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        inorder_map = {val:i for i,val in enumerate(inorder)}        
            
        def solver(low,high):
            if low>high:
                return None
            root = TreeNode(preorder.pop(0))
            index = inorder_map[root.val]
            root.left = solver(low,index-1)
            root.right = solver(index+1,high)
            return root
        
        return solver(0,len(inorder_map)-1)
```
