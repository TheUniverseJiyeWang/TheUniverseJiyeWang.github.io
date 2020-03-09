---
layout:     post
title:      Solutions to Array Nums Problems in LeetCode(3)
subtitle:   LeetCode Problem #136, #350 and #66
date:       2020-03-08
author:     JYW
header-img: img/post-200306-LeetCode1.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.136 Single Number

#### Problem Statement

Given a **non-empty** array of integers, every element appears twice except for one. Find that single one.

**Note**:

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

-**Example**
```
Input: [2,2,1]
Output: 1
Input: [4,1,2,1,2]
Output: 4
```

#### Solution

I used `sort()` function to solve the problem.
```
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        nums.sort()
        for i in range(0, len(nums)//2):
            if nums[2*i] != nums[2*i+1]:
                return nums[2*i]
        return nums[len(nums)-1]
``` 
However, because of simple math, this problem can be simply solved by dataType `HashSet`.
```
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        return 2*sum(set(nums))-sum(nums)
```
If we use the concept of bit manipulation, the space complexity can be extremeply low.
```
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        a = 0
        for i in nums:
            a ^= i
        return a
```

# Problem No.350 Intersection of Two Arrays II

#### Problem Statement

Given two arrays, write a function to compute their **intersection**.

-**Example**
```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2,2]
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [4,9]
```
-**Note**:
Each element in the result should appear as many times as it shows in both arrays.
The result can be in any order.
-**Follow up**:
What if the given array is already sorted? How would you optimize your algorithm?
What if nums1's size is small compared to nums2's size? Which algorithm is better?
What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

#### Solution

The first thought is to get a list without duplicate and count each elements in both lists. Then choose the minimum count number and append this element in a new list for minimum count times.
```
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        result=[]
        n1=list(dict.fromkeys(nums1))
        n2= list(dict.fromkeys(nums2))
        if len(n1) < len(n2):
            n = n1
        else:
            n = n2
        for i in range(0,len(n)):
            c1=nums1.count(n[i])
            c2=nums2.count(n[i])
            c=min(c1,c2)
            for j in range(0,c):
                result.append(n[i])
        return result
``` 
Then another solution is to sort two lists first and the using two **pointers** to find the intersected elements.
```
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        nums1, nums2 = sorted(nums1), sorted(nums2)
        pt1 = pt2 = 0
        result = []

        while True:
            try:
                if nums1[pt1] > nums2[pt2]:
                    pt2 += 1
                elif nums1[pt1] < nums2[pt2]:
                    pt1 += 1
                else:
                    result.append(nums1[pt1])
                    pt1 += 1
                    pt2 += 1
            except IndexError:
                break
        return result
```
Or we can use the dataType `Dict` to solve the problem.
```
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        counts = {}
        result = []
        for num in nums1:
            counts[num] = counts.get(num, 0) + 1
        for num in nums2:
            if num in counts and counts[num] > 0:
                result.append(num)
                counts[num] -= 1
        return result
```
Or we can use `Counter()`, a similar concept to `Dict`. The runtime is pretty the same.
```
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        counts = collections.Counter(nums1)
        result = []
        for num in nums2:
            if counts[num] > 0:
                result += num,
                counts[num] -= 1
        return result
```

# Problem No.66 Plus One

#### Problem Statement

Given a **non-empty** array of digits representing a non-negative integer, plus one to the integer.

The digits are stored such that the most significant digit is at the head of the list, and each element in the array contain a single digit.

You may assume the integer does not contain any leading zero, except the number 0 itself.

-**Example**
```
Input: [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
```

#### Solution

It's a relatively simple question but the tricky part is to consider all extreme conditions.
```
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        digits[-1] = digits[-1]+1
        lenth = len(digits)
        for i in range(1, lenth+1):
            if digits[-i] == 10:
                if i == lenth and digits[0] == 10:
                    digits[0] = 0
                    digits = [1] + digits
                else:
                    digits[-i] = 0
                    digits[-i-1] = digits[-i-1]+1
        return digits
```