---
layout:     post
title:      Solutions to Array Nums Problems in LeetCode(4)
subtitle:   LeetCode Problem #04, #283 and #01
date:       2020-03-10
author:     JYW
header-img: img/post-200310-LeetCode1.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.04 Median of Two Sorted Arrays

#### Problem Statement

There are two sorted arrays `nums1` and `nums2` of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

You may assume `nums1` and `nums2` cannot be both empty.

-**Example**
```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0

nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

#### Solution

This problem can be easily solved using two pointers. But the tricky part is that there are lots of conditions needed to be taken into consideration, such as odd or even numbers of elements in total, and what if there isn't any intersection range of two lists. I used three while loops to include all these conditions.
```
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        pt1 = 0
        pt2 = 0
        l1 = len(nums1)
        l2 = len(nums2)
        nums3 = []
        while len(nums3) <= (l1+l2)//2 and pt1 < l1 and pt2 < l2:
            if pt1 < l1 and nums1[pt1] <= nums2[pt2]:
                nums3.append(nums1[pt1])
                pt1 += 1
            else:
                nums3.append(nums2[pt2])
                pt2 += 1
        while len(nums3) <= (l1+l2)//2 and pt1 < l1:
            nums3.append(nums1[pt1])
            pt1 += 1
        while len(nums3) <= (l1+l2)//2 and pt2 < l2:
            nums3.append(nums2[pt2])
            pt2 += 1
        if (l1+l2)%2 == 1:
            return nums3[-1]
        else:
            return (nums3[-1]+nums3[-2])/2
``` 

# Problem No.283 Move Zeroes

#### Problem Statement

Given an array nums, write a function to move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

-**Example**
```
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```
-**Note**:
1. You must do this **in-place** without making a copy of the array.
2. Minimize the total number of operations.

#### Solution

This problem can be also solved by two pointers. All elements before the first pointer are non-zeros and all elements between two pointers are zeros. When the second pointer move to the end of the list, all zeros will be moved to the last.
```
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        pos = 0
        for i in range(len(nums)):
            el = nums[i]
            if el != 0:
                nums[pos], nums[i] = nums[i], nums[pos]
                pos += 1
``` 

# Problem No.01 Two Sum

#### Problem Statement

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **exactly** one solution, and you may not use the same element twice.

-**Example**
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

#### Solution

It's a relatively simple question that we can check the left part of target minus each element to see if there is a number inside the list, then I just need to find where it is.
```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(0,len(nums)):
            left = target - nums[i]
            if left in nums:
                for j in range(i+1, len(nums)):
                    if nums[j] == left:
                        return [i, j]
        return []
```

Or we can use dataType `dict`.
```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        seen = {}
        for i, v in enumerate(nums):
            remaining = target - v
            if remaining in seen:
                return [seen[remaining], i]
            seen[v] = i
        return []
```