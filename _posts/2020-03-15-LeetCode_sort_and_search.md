---
layout:     post
title:      Solutions to Sort and Search Problems in LeetCode(1)
subtitle:   LeetCode Problem No.88 and No.278
date:       2020-03-15
author:     JYW
header-img: img/post-200315-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.88 Merge Sorted Array

#### Problem Statement

Given two sorted integer arrays `nums1` and `nums2`, merge `nums2` into `nums1` as one sorted array.

-**Note**:
1. The number of elements initialized in `nums1` and `nums2` are `m` and `n` respectively.
2. You may assume that `nums1` has enough space (size that is greater or equal to m + n) to hold additional elements from `nums2`.

-**Example**
```
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

Output: [1,2,2,3,5,6]
```
-**Clarification**:
What should we return when `needle` is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when needle is an empty string. This is consistent to C's `strstr()` and Java's `indexOf()`.

#### Solution

The problem can be easily solved by two pointers. But if I use `nums1 =` instead of `nums1[:] = `, it won't change the actual `nums1` list.
```
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        i = 0
        j = 0
        while i < m+j and j < n:
            if nums2[j] < nums1[i]:
                nums1[:] = nums1[:i] + [nums2[j]] + nums1[i:]
                j += 1
            else:
                i += 1
        if j < n:
            nums1[:] = nums1[:i] + nums2[j:]
        nums1[:] = nums1[:m+n]
``` 
Or we can do it backwards.
```
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        while m > 0 and n > 0:
            if nums1[m-1] >= nums2[n-1]:
                nums1[m+n-1] = nums1[m-1]
                m -= 1
            else:
                nums1[m+n-1] = nums2[n-1]
                n -= 1
        if n > 0:
            nums1[:n] = nums2[:n]
```

# Problem No.278 First Bad Version

#### Problem Statement

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which will return whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

-**Example**
```
Given n = 5, and version = 4 is the first bad version.

call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true

Then 4 is the first bad version. 
```

#### Solution

It's very easy to implement binary search with `O(log(n))` complexity.
```
class Solution:
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
        r = n-1
        l = 0
        while(l<=r):
            mid = l + (r-l)//2
            if isBadVersion(mid)==False:
                l = mid+1
            else:
                r = mid-1
        return l
``` 
