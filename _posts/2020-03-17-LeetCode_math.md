---
layout:     post
title:      Solutions to Math Problems in LeetCode(1)
subtitle:   LeetCode Problem No.412 and No204
date:       2020-03-17
author:     JYW
header-img: img/post-200317-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.412 Fizz Buzz

#### Problem Statement

Write a program that outputs the string representation of numbers from 1 to n.

But for multiples of three it should output “Fizz” instead of the number and for the multiples of five output “Buzz”. For numbers which are multiples of both three and five output “FizzBuzz”.

-**Example**：
```
n = 15,

Return:
[
    "1",
    "2",
    "Fizz",
    "4",
    "Buzz",
    "Fizz",
    "7",
    "8",
    "Fizz",
    "Buzz",
    "11",
    "Fizz",
    "13",
    "14",
    "FizzBuzz"
]
```

#### Solution

It's a very simple question using `%`.
```
class Solution:
    def fizzBuzz(self, n: int) -> List[str]:
        result = []
        for i in range(1, n+1):
            if i%5 == 0 and i%3 == 0:
                result.append("FizzBuzz")
            elif i%5 == 0:
                result.append("Buzz")
            elif i%3 == 0:
                result.append("Fizz")
            else:
                result.append(str(i))
        return result
``` 
Or we can do it this way.
```
class Solution:
    def fizzBuzz(self, n: int) -> List[str]:
        result = []
        for i in range(1, n+1):
            tmp_str = ""
            if i%3 == 0:
                tmp_str += "Fizz"
            if i%5 == 0:
                tmp_str += "Buzz"
            if tmp_str == "":
                tmp_str += str(i)
            result.append(tmp_str)
        return result
```

# Problem No.204 Count Primes

#### Problem Statement

Count the number of prime numbers less than a non-negative number, **n**.

-**Example**
```
Input: 10
Output: 4
Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
```

#### Solution

If we check the prime number manually as following, the time complexity could be very large with `O(nxn)`.
```
class Solution:
    def countPrimes(self, n: int) -> int:
        if n <= 2:
            return 0
        primes = [2]
        for i in range(3,n):
            j = 0
            while j < len(primes):
                if i%primes[j] == 0:
                    break
                if j == len(primes)-1 and i%primes[j] != 0:
                    primes.append(i)
                j += 1
        return len(primes)
``` 
However, we can do it in a math way.
```
class Solution:
    def countPrimes(self, n: int) -> int:
        if n < 2:
            return 0
        s = [1] * n
        s[0] = s[1] = 0
        for i in range(2, int(n ** 0.5) + 1):
            if s[i] == 1:
                s[i * i:n:i] = [0] * len(s[i * i:n:i])
        return sum(s)
```
