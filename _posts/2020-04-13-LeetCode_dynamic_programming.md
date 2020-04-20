---
layout:     post
title:      Solutions to Dynamic Programming Problems in LeetCode(4)
subtitle:   LeetCode Problem No.152, No.309 and No.279
date:       2020-04-13
author:     JYW
header-img: img/post-200413-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.152 Maximum Product Subarray

#### Problem Statement

Given an integer array `nums`, find the contiguous subarray within an array (containing at least one number) which has the largest product.

-**Example**：
```
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.

Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

#### Solution

It's very similar to the max sum of continuous subarray. The only difference is we need to think about the negative condition, so we use a `current_min` to store the value.
```
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        current_max=nums[0]
        current_min=nums[0]
        max_val=current_max
        for i in range(1,len(nums)):
            if nums[i]>=0:
                current_max=max(nums[i],nums[i]*current_max)
                current_min=min(nums[i],nums[i]*current_min)
            if nums[i]<0:
                temp=max(nums[i],nums[i]*current_min)
                current_min=min(nums[i],nums[i]*current_max)
                current_max=temp
            max_val=max(current_max,max_val)
        return max_val
``` 

# Problem No.309 Best Time to Buy and Sell Stock with Cooldown

#### Problem Statement

Say you have an array for which the *i*th element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

1. You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
2. After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)

-**Example**：

```
Input: [1,2,3,0,2]
Output: 3 
Explanation: transactions = [buy, sell, cooldown, buy, sell]
```

#### Solution

I feel whatever stock buying and selling problem it is, it can always be solved in an `O(n)` time complexity. I don't quite understand how to figure out this solution but it can perfectly solve the problem in a extremly short time. I will write more if I finally get it.
```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices:
            return 0
        
        b, s, h = prices[0], 0, 0
        
        for p in prices:
            b = min(b, p - h)
            h = s
            s = max(s, p - b)
            
        return s
``` 

After checking on vedio, it's a problem about state trasition equation. There are three states which are hold, sold and rest. Each state can be trasited to from one or two previous states. Therefore, we can use constant space and one loop to traverse and find the answer. So, the last solution is also using the same concept except for the states are different.
```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices:
            return 0
        hold = -float('inf')
        rest = 0
        sold = 0
        for p in prices:
            prev_sold = sold
            sold = hold + p
            hold = max(hold, rest-p)
            rest = max(rest, prev_sold)
        return max(sold,rest)
```

# Problem No.279 Perfect Squares

#### Problem Statement

Given a positive integer *n*, find the least number of perfect square numbers (for example, `1`, `4`, `9`, `16`, ...) which sum to n.

-**Example**：

```
Input: n = 12
Output: 3 
Explanation: 12 = 4 + 4 + 4.

Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.
```

#### Solution

At first, I didn't think it's a dynamic programming problem but soon I came out the condition where *n* is `18`. If we start with the largest square which is 16, we can fill the rest value of `2` with two squares of `1`. However, `18` can be also achieved by two squares of `9` which is less than previous number of squares.

Therefore, we creat a list of coins that can be used to make up `n`. And using each coin, we can update the `dp` list which represents the min number of coins to make up the number of index. By traversing all coins to update the `dp` list, the last element is the least number of coins which can make up `n`.
```
class Solution:
    def numSquares(self, n: int) -> int:
        coins = [i**2 for i in range(1,int(n**0.5)+1)]
        dp = [float("inf")] * (n + 1)
        dp[0] = 0
        for c in coins:
            for i in range(1,n+1):
                if i >= c:
                    dp[i] = min(dp[i-c] + 1, dp[i])
        return dp[-1]
```
