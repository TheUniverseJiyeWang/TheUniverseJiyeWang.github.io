---
layout:     post
title:      Solutions to Dynamic Programming Problems in LeetCode(1)
subtitle:   LeetCode Problem No.70 and No.121
date:       2020-03-19
author:     JYW
header-img: img/post-200319-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.70 Climbing Stairs

#### Problem Statement

You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

-**Note**:

Given `n` will be a positive integer.

-**Example**：
```
Input: 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

Input: 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

#### Solution

At first, my thought is to solve this problem recursively. The last step can be one or two, so the number of combinations of `n` steps should be sum of the numbers of combinations of `n-1` steps and `n-2` steps. And we can keep track on the previous calculation in a `dict` to reduce the time complexity.
```
class Solution:
    def climbStairs(self, n: int) -> int:
        
        log = {}
        
        def climb_one_or_two(i: int, n: int) -> int:
            if i > n:
                return 0
            if i == n:
                return 1
            if n-i not in log:
                log[n-i] = climb_one_or_two(i+1, n) + climb_one_or_two(i+2, n)
            else:
                return log[n-i]
            return climb_one_or_two(i+1, n) + climb_one_or_two(i+2, n)
        
        return climb_one_or_two(0, n)
``` 
Or now that we know the rule, we can also do it in a math way.
```
class Solution:
    def climbStairs(self, n: int) -> int:
        steps = [0 for i in range(0, n+2)]
        steps[1] = 1
        steps[2] = 2
        j = 3
        while j <= n:
            steps[j] = steps[j-1] + steps[j-2]
            j += 1
        return steps[n]
```

# Problem No.121 Best Time to Buy and Sell Stock

#### Problem Statement

Say you have an array for which the `i`th element is the price of a given stock on day `i`.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

-**Example**
```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.

Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

#### Solution

We can definately use multiple indicators to solve the problem with `O(n)` time complexity. 
```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if prices == []:
            return 0
        profit = 0
        minprice = prices[0]
        l = len(prices)
        idx = 0
        while idx < l:
            if prices[idx] - minprice > profit:
                profit = prices[idx] - minprice
            if prices[idx] < minprice:
                minprice = prices[idx]
            idx += 1
        return profit
``` 
