---
layout:     post
title:      Solutions to Math Problems in LeetCode(4)
subtitle:   LeetCode Problem No.171, No.50 and No.69
date:       2020-04-04
author:     JYW
header-img: img/post-200404-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.171 Excel Sheet Column Number

#### Problem Statement

Given a column title as appear in an Excel sheet, return its corresponding column number.

For example:
```
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
    ...
```

-**Example**：
```
Input: "A"
Output: 1

Input: "AB"
Output: 28

Input: "ZY"
Output: 701
```

#### Solution

It's easy to solve the problem by `ord()`.
```
class Solution:
    def titleToNumber(self, s: str) -> int:
        res = 0
        for ch in s:
            c = ord(ch) - 64
            res = res*26 + c
        return res
```

# Problem No.50 Power(x, n)

#### Problem Statement

Implement `pow(x, n)`, which calculates x raised to the power `n(x**n)`.

-**Example**:
```
Input: 2.00000, 10
Output: 1024.00000

Input: 2.10000, 3
Output: 9.26100

Input: 2.00000, -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
```

-**Note**:

1. -100.0 < x < 100.0
2. n is a 32-bit signed integer, within the range [−231, 231 − 1]

#### Solution

I solved the problem by multiple the result itself for log2(n) times.
```
class Solution:
    def myPow(self, x: float, n: int) -> float:
        more = 1
        if n > 0:
            res = x
        elif n < 0:
            res = 1/x
        else:
            return 1
        n = abs(n)
        while n != 1:
            if n%2 == 1:
                more *= res
            res *= res
            n = n//2
        res *= more
        return res
``` 
We can also do it recursively.
```
class Solution:
    def myPow(self, x: float, n: int) -> float:

        def recurse(x, n):
            if n == 1:
                return x
            if n % 2 == 0:
                n = n // 2
                temp = recurse(x, n)
                result = temp * temp
            else:
                n = (n-1) // 2
                temp = recurse(x, n)
                result = temp * temp * x
            return result
        
        if n > 0:
            return recurse(x, n)
        elif n == 0:
            return 1
        else:
            return 1 / recurse(x, -n)
```

# Problem No.69 Sqrt(x)

#### Problem Statement

Implement int `sqrt(int x)`.

Compute and return the square root of x, where x is guaranteed to be a non-negative integer.

Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.

-**Example**:
```
Input: 4
Output: 2

Input: 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since 
             the decimal part is truncated, 2 is returned.
```

#### Solution

We can solve the problem by binary search.
```
class Solution:
    def mySqrt(self, x: int) -> int:
        left, right = 0, x
        res = 0
        while left <= right:
            mid = (right + left) // 2
            if mid * mid <= x:
                res = mid
                left = mid + 1
            else:
                right = mid - 1
        return res
```
