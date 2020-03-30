---
layout:     post
title:      Solutions to Dynamic Programming Problems in LeetCode(3)
subtitle:   LeetCode Problem No.55 and No.62
date:       2020-03-30
author:     JYW
header-img: img/post-200330-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.55 Jump Game

#### Problem Statement

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

-**Example**：
```
Input: [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.

Input: [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum
             jump length is 0, which makes it impossible to reach the last index.
```

#### Solution

Firstly, I checked with any mumber less than 1, which means the index can't proceed. Then I checked if there is a previous number than can jump over the point. If there is no number that can jump over and proceed, then return `False`.
```
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        idx = 0
        while idx < len(nums) - 1:
            if nums[idx] < 1:
                tmp = idx
                tmp = tmp - 1
                while tmp >= 0:
                    if nums[tmp] > idx - tmp:
                        break
                    tmp -= 1
                else:
                    return False
            idx += 1
        return True
``` 
Or we can check each number from the end of the list to see if it can move to the first number.
```
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        l = len(nums)
        end = l-1
        i= end
        while i >= 0:
            if i + nums[i] >= end:
                end = i
            i -= 1
        return True if end == 0 else False
```
Or we can check from the start to see if there is a break point that can't proceed.
```
class Solution:
    def canJump(self, nums: [int]) -> bool:
        life = 1
        for num in nums[:-1]:
            life = max(life - 1, num)

            if life == 0:
                return False

        return True
```

# Problem No.62 Unique Paths

#### Problem Statement

A robot is located at the top-left corner of a *m x n* grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

![Unique Paths](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

-**Example**
```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right

Input: m = 7, n = 3
Output: 28
```

-**Constraints**:

`1 <= m, n <= 100`
It's guaranteed that the answer will be less than or equal to `2 * 10 ^ 9`.

#### Solution

We can use a matrix to represent the unique paths to current location. The first row and first column all contain the value `1` which means there is only 1 way to get there. And each sqaure of other part is the sum of its top square and left sqaure. Then we just need to walk throught the matrix and find the answer in the last sqaure. 
```
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[0]*n for _ in range(m)]
        for j in range(n):
            dp[0][j] = 1
        for i in range(m):
            dp[i][0] = 1
        for i in range(1,m):
            for j in range(1,n):
                dp[i][j] = dp[i-1][j]+dp[i][j-1]
        return dp[-1][-1]
``` 
