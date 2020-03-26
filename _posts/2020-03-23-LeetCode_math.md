---
layout:     post
title:      Solutions to Math Problems in LeetCode(2)
subtitle:   LeetCode Problem No.326 and No.13
date:       2020-03-23
author:     JYW
header-img: img/post-200323-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.326 Power of Three

#### Problem Statement

Given an integer, write a function to determine if it is a power of three.

-**Example**：
```
Input: 27
Output: true

Input: 0
Output: false

Input: 9
Output: true

Input: 45
Output: false
```

-**Follow up**:

Could you do it without using any loop / recursion?

#### Solution

It can be easily solved by loop.
```
class Solution:
    def isPowerOfThree(self, n: int) -> bool:
        powers = [1]
        tmp = 3
        while tmp <= n:
            powers.append(tmp)
            tmp *= 3
        if n not in powers:
            return False
        else:
            return True
``` 
We can also convert the number to base 3 and then use Regex to match with `10*$`.
```
class Solution:
    def isPowerOfThree(self, n: int) -> bool:
        if n < 1:
            return False
        
        def toBaseThree(t: int) -> str:
            digits = ['0', '1', '2']
            index = []
            result = []
            while True:
                s = t//3
                y = t%3
                index = index + [y]
                if s == 0:
                    break
                t = s
            index.reverse()
            for idx in index:
                result.append(digits[idx])
            return ''.join(result)
        
        n_str = toBaseThree(n)
        match = re.match( r'10*$', n_str, re.M|re.I)
        if match:
            return True
        else:
            return False
```
We can also do it using `log()`. However, because of the float problem in Python, it can't properly check the int.

# Problem No.13 Roman to Integer

#### Problem Statement

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, two is written as `II` in Roman numeral, just two one's added together. Twelve is written as, `XII`, which is simply `X` + `II`. The number twenty seven is written as `XXVII`, which is `XX` + `V` + `II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

`I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
`X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
`C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.
Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

-**Example**
```
Input: "III"
Output: 3

Input: "IV"
Output: 4

Input: "IX"
Output: 9

Input: "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.

Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

#### Solution

It's easy to reverse the string and calculate by each character.
```
class Solution:
    def romanToInt(self, s: str) -> int:
        dic = {'I':1,'V':5,'X':10,'L':50,'C':100,'D':500,'M':1000}
        s = list(s)
        s.reverse()
        cur = 1
        result = 0
        for el in s:
            if dic[el] < cur:
                result -= dic[el]
            else:
                result += dic[el]
                cur = dic[el]
        return result
``` 