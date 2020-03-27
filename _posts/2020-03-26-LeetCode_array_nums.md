---
layout:     post
title:      Solutions to Array Nums Problems in LeetCode(6)
subtitle:   LeetCode Problem No.15 and No.73
date:       2020-03-26
author:     JYW
header-img: img/post-200326-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.15 3Sum

#### Problem Statement

Given an array `nums` of n integers, are there elements a, b, c in `nums` such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

-**Note**:

The solution set must not contain duplicate triplets.

-**Example**
```
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

#### Solution

It's easy to develop a solution with loops.
```
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        l = len(nums)
        nums.sort()
        res = []
        if l < 3:
            return []
        for i in range(0,l-2):
            j = i + 1
            while j < l-1:
                if 0-nums[i]-nums[j] in nums[j+1:]:
                    tmp = [nums[i],nums[j],0-nums[i]-nums[j]]
                    if tmp not in res:
                        res.append(tmp)
                j += 1
        return res
``` 
Or a optimized way.
```
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        res = []
        nums.sort()
        for i in range(len(nums)-2):
            if i > 0 and nums[i] == nums[i-1]:
                continue
            if nums[i] > 0:
                break
            l, r = i+1, len(nums)-1
            while l < r:
                s = nums[i] + nums[l] + nums[r]
                if s < 0:
                    l +=1 
                elif s > 0:
                    r -= 1
                else:
                    res.append((nums[i], nums[l], nums[r]))
                    while l < r and nums[l] == nums[l+1]:
                        l += 1
                    while l < r and nums[r] == nums[r-1]:
                        r -= 1
                    l += 1; r -= 1
        return res
```

# Problem No.73 Set Matrix Zeroes

#### Problem Statement

Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it **in-place**.

-**Example**
```
Input: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
Output: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]

Input: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
Output: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
```

-**Follow up**:

1. A straight forward solution using `O(mn)` space is probably a bad idea.
2. A simple improvement uses `O(m + n)` space, but still not the best solution.
3. Could you devise a constant space solution?

#### Solution

If we don't consider the space to store existing zeros, we can definitely solve the problem using loops.
```
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        zeros = []
        for i in range(0,len(matrix)):
            for j in range(0,len(matrix[0])):
                if matrix[i][j] == 0:
                    zeros.append((i,j))
        for each in zeros:
            x_z, y_z = each[0], each[1]
            for x in range(len(matrix)):
                matrix[x][y_z] = 0
            for y in range(len(matrix[0])):
                matrix[x_z][y] = 0
``` 
We can also use the first column and first row to set all elements in the matrix, making the time complexity `O(mxn)` and space complexity `O(1)`.
```
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        is_col = False
        R = len(matrix)
        C = len(matrix[0])
        for i in range(R):
            # Since first cell for both first row and first column is the same i.e. matrix[0][0]
            # We can use an additional variable for either the first row/column.
            # For this solution we are using an additional variable for the first column
            # and using matrix[0][0] for the first row.
            if matrix[i][0] == 0:
                is_col = True
            for j in range(1, C):
                # If an element is zero, we set the first element of the corresponding row and column to 0
                if matrix[i][j]  == 0:
                    matrix[0][j] = 0
                    matrix[i][0] = 0

        # Iterate over the array once again and using the first row and first column, update the elements.
        for i in range(1, R):
            for j in range(1, C):
                if not matrix[i][0] or not matrix[0][j]:
                    matrix[i][j] = 0

        # See if the first row needs to be set to zero as well
        if matrix[0][0] == 0:
            for j in range(C):
                matrix[0][j] = 0

        # See if the first column needs to be set to zero as well        
        if is_col:
            for i in range(R):
                matrix[i][0] = 0
```
