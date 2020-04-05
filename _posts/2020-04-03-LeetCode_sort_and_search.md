---
layout:     post
title:      Solutions to Sort and Search Problems in LeetCode(3)
subtitle:   LeetCode Problem No.162, No.34 and No.56
date:       2020-04-03
author:     JYW
header-img: img/post-200403-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.162 Find Peak Element

#### Problem Statement

A peak element is an element that is greater than its neighbors.

Given an input array `nums`, where `nums[i] ≠ nums[i+1]`, find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that `nums[-1] = nums[n] = -∞`.

-**Example**
```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.

Input: nums = [1,2,1,3,5,6,4]
Output: 1 or 5 
Explanation: Your function can return either index number 1 where the peak element is 2, 
             or index number 5 where the peak element is 6.
```
-**Note**:

Your solution should be in logarithmic complexity.

#### Solution

Luckily, we have infinite number in Python and by simply adding two infinite numbers to the start and end, we can solve the problem.
```
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        a = -float('inf')
        tmp = [a]+nums+[a]
        for i in range(1,len(nums)+1):
            if tmp[i] > tmp[i-1] and tmp[i] > tmp[i+1]:
                return i-1
``` 
Or we can use iterative binary search. But it seems pointless since the list is not sorted?
```
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        i = 0
        l = len(nums) - 1
        while i < l:
            mid = (i + l)//2
            if nums[mid] > nums[mid + 1]:
                l = mid
            else:
                i = mid + 1
        return i
```

# Problem No.34 Find First and Last Position of Element in Sorted Array

#### Problem Statement

Given a collection of intervals, merge all overlapping intervals.

-**Example**

```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].

Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

#### Solution

We can first find the target by binary search and then expand from the position.
```
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        if nums == [] or nums[0] > target or target > nums[-1]:
            return [-1, -1]
        hi = len(nums)
        lo = 0
        while lo < hi:
            mid = (lo + hi)//2
            if nums[mid] > target:
                hi = mid
            elif nums[mid] < target:
                lo = mid + 1
            else:
                break
        if nums[mid] != target:
            return [-1, -1]
        
        start = mid
        end = mid
        res = [0, len(nums)-1]
        while start >= 0:
            if nums[start] != target:
                res[0] = start+1
                break
            start -= 1
        while end < len(nums):
            if nums[end] != target:
                res[1] = end - 1
                break
            end += 1
        return res
``` 
Or in a much more delicate way.
```
class Solution:
    def extreme_insertion_index(self, nums, target, left):
        lo = 0
        hi = len(nums)

        while lo < hi:
            mid = (lo + hi) // 2
            if nums[mid] > target or (left and target == nums[mid]):
                hi = mid
            else:
                lo = mid+1

        return lo


    def searchRange(self, nums, target):
        left_idx = self.extreme_insertion_index(nums, target, True)
        if left_idx == len(nums) or nums[left_idx] != target:
            return [-1, -1]

        return [left_idx, self.extreme_insertion_index(nums, target, False)-1]
```

# Problem No.56 Merge Intervals

#### Problem Statement

Given a collection of intervals, merge all overlapping intervals.

-**Example**

```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].

Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

#### Solution

We can first sort the list by the first elements and then check if there is any merging operation.
```
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        if len(intervals) < 2:
            return intervals
        intervals.sort(key=lambda x: x[0])
        i = 1
        l = len(intervals)
        while i < l:
            if min(intervals[i][0],intervals[i][1]) > max(intervals[i-1][0],intervals[i-1][1]):
                i += 1
            else:
                intervals[i-1] = [min(intervals[i-1][0],intervals[i][0]),max(intervals[i-1][1],intervals[i][1])]
                intervals.pop(i)
                l -= 1
        return intervals
```
