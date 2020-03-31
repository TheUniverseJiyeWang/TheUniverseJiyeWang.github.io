---
layout:     post
title:      Solutions to Math Problems in LeetCode(3)
subtitle:   LeetCode Problem No.202 and No.172
date:       2020-03-31
author:     JYW
header-img: img/post-200331-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.202 Happy Number

#### Problem Statement

Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

-**Example**：
```
Input: 19
Output: true
Explanation: 
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

#### Solution

We can use recursive solution to check if the square sum of each digits is 1 or creates a loop.
```
class Solution:
    def isHappy(self, n: int) -> bool:
        
        al_res = [n]
        
        def isHappyAll(n: int, al_res: [int]):
            res = 0
            for each in str(n):
                res += int(each)**2
            if res == 1:
                return True
            elif res in al_res:
                return False
            else:
                al_res.append(res)
                return isHappyAll(res, al_res)
            
        return isHappyAll(n, al_res)
``` 
Or we can use a `set()` to end the loop.
```
class Solution:
    def isHappy(self, n: int) -> bool:
        def nextNumber(n: int) -> int:
            x = 0
            while n:
                x += (n % 10) ** 2
                n //= 10
            return x
        
        seen = set()
        while n not in seen:
            seen.add(n)
            n = nextNumber(n)
            
        return n == 1
```
Or we can limit the time of recursion.
```
class Solution:
    def isHappy(self, n: int) -> bool:
        k = 0
        a = 0
        for i in range(32):
            while n:
                k = n%10
                a += k * k
                n = n//10
            if a == 1:
                return True
            n = a
            a = 0
        return False
```

# Problem No.172 Factorial Trailing Zeroes

#### Problem Statement

Given an integer *n*, return the number of trailing zeroes in *n!*.

-**Example**:
```
Input: 3
Output: 0
Explanation: 3! = 6, no trailing zero.

Input: 5
Output: 1
Explanation: 5! = 120, one trailing zero.
```

-**Note**:

Your solution should be in logarithmic time complexity.

#### Solution

By simple math, we can find that how many zeroes by checking how many 5 recursivly. For example, if there is one 5, there should be one 0, and if there is one 25, there is additionally one 0. So here we go.
```
class Solution:
    def trailingZeroes(self, n: int) -> int:
        res = 0
        while n > 0:
            res += n//5
            n = n//5
        return res
``` 
