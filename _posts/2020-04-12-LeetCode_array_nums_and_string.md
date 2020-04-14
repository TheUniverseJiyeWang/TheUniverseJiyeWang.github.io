---
layout:     post
title:      Solutions to Array Nums Problems in LeetCode(11)
subtitle:   LeetCode Problem No.334 and No.238
date:       2020-04-06
author:     JYW
header-img: img/post-200406-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.334 Increasing Triplet Subsequence

#### Problem Statement


Given an unsorted array return whether an increasing subsequence of length 3 exists or not in the array.

Formally the function should:
```
Return true if there exists i, j, k
such that arr[i] < arr[j] < arr[k] given 0 ≤ i < j < k ≤ n-1 else return false.
```

-**Note**:

Your algorithm should run in O(n) time complexity and O(1) space complexity.

-**Example**
```
Input: [1,2,3,4,5]
Output: true

Input: [5,4,3,2,1]
Output: false
```

#### Solution

Because we only need to consider three element, we can use two consitant spaces to store two value named as min and max. Then when we traverse the list, we can renew two values by conditions and if we find an element larger than these two values (if the two values are not equal), we can say there is a triplet increasing subsequence. 
```
class Solution:
    def increasingTriplet(self, nums: List[int]) -> bool:
        if len(nums)<3:
            return False
    
        min_v=max_v=nums[0]

        for i in range(1,len(nums)):
            if min_v==max_v and nums[i]<max_v:
                min_v=max_v=nums[i]
            elif nums[i]>max_v:
                if max_v>min_v:
                    return True
                else:
                    max_v=nums[i]
            elif nums[i]<max_v:
                if nums[i]>min_v:
                    max_v=nums[i]
                else:
                    min_v=nums[i]
    
        return False
``` 

# Problem No.238 Product of Array Except Self

#### Problem Statement

Given an array `nums` of *n* integers where *n* > 1,  return an array `output` such that `output[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

-**Example**
```
Input:  [1,2,3,4]
Output: [24,12,8,6]
```
-**Constraints**:

It's guaranteed that the product of the elements of any prefix or suffix of the array (including the whole array) fits in a 32 bit integer.

-**Note**:

Please solve it without division and in `O(n)`.

-**Follow up**:

Could you solve it with constant space complexity? (The output array **does not** count as extra space for the purpose of space complexity analysis.)

#### Solution

If we don't think of the constraints, we can easily find the solution.
```
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        res = 1
        count = 0
        for each in nums:
            if each == 0:
                count += 1
            else:
                res *= each
        if count > 1:
            zero = 0
            res = 0
        if count == 1:
            zero = res
            res = 0
        result = []
        for each in nums:
            if each != 0:
                result.append(int(res/each))
            else:
                result.append(zero)
        return result
``` 

A better solution is to use two list to store the product before and after each element in the origin list. Then we just need to calculate the product of the *i*th elements in these two list and the answer is the *i*th output.
```
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        length = len(nums)
        L, R, answer = [0]*length, [0]*length, [0]* length
        L[0] = 1
        for i in range(1,length):
            L[i] = nums[i-1]*L[i-1]
        R[length-1] = 1
        for i in reversed(range(length - 1)):
            R[i] = nums[i+1] * R[i + 1]
        
        for i in range(length):
            answer[i] = L[i] * R[i]
        
        return answer
```

We can even optimize the solution to `O(1)` space complexity.
```
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        length = len(nums)
        answer = [0] *length
        answer[0] = 1
        for i in range(1, length):
            answer[i] = nums[i-1] * answer[i-1]
        r = 1
        for i in reversed(range(length)):
            answer[i] = answer[i]*r
            r *= nums[i]
        return answer
```
