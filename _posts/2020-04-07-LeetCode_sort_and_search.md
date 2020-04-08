---
layout:     post
title:      Solutions to Sort and Search Problems in LeetCode(4)
subtitle:   LeetCode Problem No.162 and No.240
date:       2020-04-07
author:     JYW
header-img: img/post-200407-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.33 Search in Rotated Sorted Array

#### Problem Statement

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).

You are given a target value to search. If found in the array return its index, otherwise return `-1`.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of `O(log n)`.

-**Example**
```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4

Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

#### Solution

We can first find from where the list is rotated and then decide where to search for the target.
```
class Solution:
    def binarySearch(self, nums, l, r, target):
        if not nums:
            return -1
        while l<=r:
            mid = (l+r)//2
            if nums[mid]==target:
                return mid
            if nums[mid]>target:
                r = mid-1
            else:
                l = mid+1
        return -1
    
    
    def search(self, nums: List[int], target: int) -> int:
        
        if not nums:
            return -1
        
        rotatedAt = None
        
        l = 0
        r = len(nums)-1
        
        while l<=r:
            
            mid = (l+r)//2
            print(mid, l, r)
            if mid-1>=0:
                if nums[mid-1]>nums[mid]:
                    rotatedAt = mid
                    break
            
            if nums[mid]<nums[-1]:
                r =  mid-1
            else:
                l = mid+1
                
        if rotatedAt == None:
            return self.binarySearch(nums, 0, len(nums)-1, target)
        else:
            val = self.binarySearch(nums, 0, rotatedAt-1, target)
            if val==-1:
                return self.binarySearch(nums, rotatedAt, len(nums)-1, target)
            return val
``` 

# Problem No.240 Search a 2D Matrix II

#### Problem Statement

Write an efficient algorithm that searches for a value in an *m* x *n* matrix. This matrix has the following properties:

1. Integers in each row are sorted in ascending from left to right.
2. Integers in each column are sorted in ascending from top to bottom.

-**Example**

Consider the following matrix:

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

Given target = `5`, return `true`.
Given target = `20`, return `false`.

#### Solution

We can first find the corner point where the value is larger than the target and the search from there. But we can also find the upper limit of rows and cols as well.
```
class Solution:
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if matrix == [] or matrix[0] == []:
            return False
        
        rows = len(matrix)-1
        cols = len(matrix[0])-1
        row = 0
        col = 0
        if matrix[row][col] > target:
                return False
        
        while 0 <= row < rows or 0 <= col < cols:
            if matrix[row][col] < target:
                row = min(row+1, rows)
                col = min(col+1, cols)
            else:
                break
        
        print(row)
        print(col)
        for i in range(0, rows+1):
            for j in range(col, cols+1):
                if matrix[i][j] == target:
                    return True
        
        for i in range(0, cols+1):
            for j in range(row, rows+1):
                if matrix[j][i] == target:
                    return True
            
        return False
``` 

We can also search from the row and then search from the col by constraints. But the time complexity is `O(m+n)`.
```
class Solution:
    def searchMatrix(self, matrix, target):
        m = len(matrix)
        if m < 1:
            return False
        n = len(matrix[0])
        if n < 1:
            return False
        row, col = 0, n - 1
        while row < m and col >= 0:
            curr = matrix[row][col]
            if target > curr:
                row += 1
            elif target < curr:
                col -= 1
            else:
                return True
        return False
```

Also, we can apply binary search to make it even faster with time complexity `O(m log n)`.
```
class Solution:
    def searchMatrix(self, matrix, target):
        m = len(matrix)
        if m < 1:
            return False
        n = len(matrix[0])
        if n < 1:
            return False
        i = 0
        while i < m and target >= matrix[i][0]:
            low, high = 0, n
            while low < high:
                mid = (low+high)//2
                if target > matrix[i][mid]:
                    low = mid + 1
                elif target < matrix[i][mid]:
                    high = mid
                else:
                    return True
            i += 1
        return False
```
