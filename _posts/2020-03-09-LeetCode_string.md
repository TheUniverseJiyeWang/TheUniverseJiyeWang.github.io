---
layout:     post
title:      Solutions to String Problems in LeetCode(1)
subtitle:   LeetCode Problem #344, #7 and #387
date:       2020-03-09
author:     JYW
header-img: img/post-200308-LeetCode1.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.344 Reverse String

#### Problem Statement

Write a function that reverses a string. The input string is given as an array of characters `char[]`.

Do not allocate extra space for another array, you must do this by **modifying the input array** in-place with O(1) extra memory.

You may assume all the characters consist of printable ascii characters.

-**Example**
```
Input: ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```

#### Solution

It's a quite simple question. I just used two pointers to solve the problem. 
```
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        for i in range(0, len(s)//2):
            changed = s[i]
            s[i] = s[-i-1]
            s[-i-1] = changed
``` 
In python, we can use `reverse()` function to achieve it but the runtime is slightly more than the manual one.

# Problem No.7 Reverse Integer

#### Problem Statement

Given a 32-bit signed integer, reverse digits of an integer.

-**Example**
```
Input: 123
Output: 321
Input: -123
Output: -321
Input: 120
Output: 21
```
-**Note**:
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

#### Solution

The first thought is to turn the dataType `int` into dataType `str` and `List[str]`. Then we can easily reverse it.
```
class Solution:
    def reverse(self, x: int) -> int:
        s = str(x)
        result = []
        l = len(s)
        for i in range(0,l):
            result.append(s[i])
        if result[0] == "-":
            for i in range(1, (l-1)//2+1):
                changed = result[i]
                result[i] = result[-i]
                result[-i] = changed
            while True:
                if result[1] == 0:
                    result = result[0] + result[2:]
                else:
                    break
        else:
            for i in range(0,l//2):
                changed = result[i]
                result[i] = result[-i-1]
                result[-i-1] = changed
            while True:
                if result[0] == 0:
                    result = result[1:]
                else:
                    break
        result = "".join(result)
        result = int(result)
        if result not in range(-2**31,2**31-1):
            return 0
        return result
``` 
It can be also done in a math way.
```
class Solution:
    def reverse(self, x: int) -> int:
        rev = 0
        neg = False
        if x < 0:
            neg = True
            x = abs(x)
        while x != 0:
            pop = x%10
            x = x//10
            rev = rev*10 +pop
        if rev not in range(-2**31,2**31-1):
            return 0
        if neg == True:
            rev = -rev
        return rev
```

# Problem No.387

#### Problem Statement

Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

-**Example**
```
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
```
-**Note**: You may assume the string contain only lowercase letters.

#### Solution

The first thought is to create a dataType `dict` to count the numbers of every characters and then find the first single character.
```
class Solution:
    def firstUniqChar(self, s: str) -> int:
        dict_c = {}
        l = len(s)
        for i in range(0,l):
            if s[i] not in dict_c:
                dict_c[s[i]] = 1
            else:
                dict_c[s[i]] += 1
        result = -1
        for i in range(0, l):
            if dict_c[s[i]] == 1:
                result = i
                break
        return result
```
