---
layout:     post
title:      Solutions to Dynamic Programming Problems in LeetCode(2)
subtitle:   LeetCode Problem No.53 and No.198
date:       2020-03-21
author:     JYW
header-img: img/post-200321-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.53 Maximum Subarray

#### Problem Statement

Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

-**Example**：
```
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

-**Follow up**:

If you have figured out the `O(n)` solution, try coding another solution using the divide and conquer approach, which is more subtle.

#### Solution

We can use two `Sum` to store and compare all the positive sums of consecutive numbers in the list.
```
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        if not nums:
            return 0

        curSum = maxSum = nums[0]
        for num in nums[1:]:
            curSum = max(num, curSum + num)
            maxSum = max(maxSum, curSum)

        return maxSum
``` 
Or we can also do that in a way that reshape the list itself, changing all the elements to sum of previous consecutive numbers if the previous sum is positive.
```
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        for i in range(1, len(nums)):
            if nums[i-1] > 0:
                nums[i] += nums[i-1]
        return max(nums)
```

# Problem No.198 House Robber

#### Problem Statement

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

-**Example**
```
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.

Input: [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
             Total amount you can rob = 2 + 9 + 1 = 12.
```

#### Solution

We can use the same solution as the last problem to reshape the list and compare the last two elements (if exist).
```
class Solution:
    def rob(self, nums: List[int]) -> int:
        if not nums:
            return 0
        l = len(nums)
        for i in range(0, l):
            if i == 2:
                nums[i] += nums[0]
            elif i > 2:
                nums[i] += max(nums[i-2],nums[i-3])
        if l == 1:
            return nums[0]
        else:
            return max(nums[-2:])
``` 
