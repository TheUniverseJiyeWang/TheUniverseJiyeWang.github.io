---
layout:     post
title:      Solutions to Dynamic Programming Problems in LeetCode(4)
subtitle:   LeetCode Problem No.322 and No.300
date:       2020-04-08
author:     JYW
header-img: img/post-200408-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.322 Coin Change

#### Problem Statement

You are given coins of different denominations and a total amount of money *amount*. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

-**Example**：
```
Input: coins = [1, 2, 5], amount = 11
Output: 3 
Explanation: 11 = 5 + 5 + 1

Input: coins = [2], amount = 3
Output: -1
```

#### Solution

Firstly, I try to use recursive function to solve the problem. But with large amount and complex coins, the run time could be extremply high.
```
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        
        if amount == 0:
            return 0
        
        coins.sort(reverse = True)
        
        def dfs(amount: int, coins: List[int]):
            if amount in coins:
                count = 1
                return True, count
            elif amount <= 0:
                return False, 0
            for coin in coins:
                res, count = dfs(amount - coin, coins)
                if res == True:
                    return True, count+1
            return False,0
        
        res, count = dfs(amount, coins)
        
        if res == False:
            return -1
        else:
            return count
``` 

Another way, we can traverse from 0 to amount and find all possible combinations with minimum number of coins.
```
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [float('inf')] * (amount + 1)
        dp[0] = 0
        
        for coin in coins:
            for x in range(coin, amount + 1):
                dp[x] = min(dp[x], dp[x - coin] + 1)
        return dp[amount] if dp[amount] != float('inf') else -1 
```

# Problem No.300 Longest Increasing Subsequence

#### Problem Statement

Given an unsorted array of integers, find the length of longest increasing subsequence.

-**Example**
```
Input: [10,9,2,5,3,7,101,18]
Output: 4 
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4. 
```

-**Note**:

1. There may be more than one LIS combination, it is only necessary for you to return the length.
2. Your algorithm should run in `O(n**2)` complexity.

-**Follow up**:

Could you improve it to `O(n log n)` time complexity?

#### Solution

The best solution is to use a function called `binarySearch()` with `O(logn)` time complexity which is `bisect.bisect_left()` in Python. The function is to find the position where a number should be insert in a sorted list. By doing so, each time we find a number that need to be insert in the list, if it should be inside the list, we update the list and make it the minmum number if it's lower than current number, and the same when it's larger. And when we find a new element that can be insert in the end of the list, we just append it to the list and make the list longer. By doing so until we traverse the whole list, we can actuall get the longest increasing subsequence directly.
```
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        dp = []
        for c in nums:
            i = bisect.bisect_left(dp, c)
            if i == len(dp):
                dp.append(c)
            else:
                dp[i] = c
        return len(dp)
``` 

