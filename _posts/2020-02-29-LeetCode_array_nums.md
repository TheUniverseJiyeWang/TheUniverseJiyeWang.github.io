---
layout:     post
title:      Solutions to Array Nums Problems in LeetCode(1)
subtitle:   LeetCode Problem No.26 and No.122
date:       2020-02-29
author:     JYW
header-img: img/post-200229-LeetCode1.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.26 Remove Duplicates from Sorted Array

#### Problem Statement

Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array** in-place with O(1) extra memory.

-**Example**
```
Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.

It doesn't matter what you leave beyond the returned length.
```

#### Solution

I use a pointer-like concept to solve the problem. When j moving down the for loop, it changes the **first i elements** in the list with different values.
![Reshaping the array nums](https://tva1.sinaimg.cn/large/00831rSTgy1gcdwe78ikvj30oa037gm5.jpg)
```
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if len(nums) <= 1:
            return len(nums)
        i = 0
        for j in range(1, len(nums)):
            if nums[i] != nums[j]:
                i += 1
                nums[i] = nums[j]
        return i+1
``` 

# Problem No.122 Best Time to Buy and Sell Stock II

#### Problem Statement

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

**Note**: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

-**Example**
```
Input: [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
             Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
```

#### Solution

Because it's impossible that the max profit in a time period exceeds the sum of max profit in separated time slots inside the same time period, so we can simply calculate the max profit by adding up all positive value between 2 days.

![All possible coditions](https://tva1.sinaimg.cn/large/00831rSTgy1gcdy45a3isj31y00lwtr7.jpg)
```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if len(prices) <= 1:
            return 0
        max_profit = 0
        for i in range(1, len(prices)):
            if prices[i] > prices[i-1]:
                max_profit += prices[i] - prices[i-1]
        return max_profit
``` 