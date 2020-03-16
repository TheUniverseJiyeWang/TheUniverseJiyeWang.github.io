---
layout:     post
title:      Solutions to String Problems in LeetCode(2)
subtitle:   LeetCode Problem No.242, No.125 and No.08
date:       2020-03-12
author:     JYW
header-img: img/post-200312-LeetCode1.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.242 Valid Anagram

#### Problem Statement

Given two strings s and t , write a function to determine if t is an anagram of s.

-**Example**
```
Input: s = "anagram", t = "nagaram"
Output: true
Input: s = "rat", t = "car"
Output: false
```
-**Note**:
You may assume the string contains only lowercase alphabets.

-**Follow up**:
What if the inputs contain unicode characters? How would you adapt your solution to such case?

#### Solution

I used dataType `dict` to solve the problem. It can also solve the case involving unicode characters.
```
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
        dict_c = {}
        l = len(s)
        for i in range(0,l):
            if s[i] not in dict_c:
                dict_c[s[i]] = 1
            else:
                dict_c[s[i]] += 1
        for i in range(0,l):
            if t[i] not in dict_c:
                return False
            else:
                dict_c[t[i]] -= 1
                if dict_c[t[i]] < 0:
                    return False
        return True
``` 

# Problem No.125 Valid Palindrome

#### Problem Statement

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

**Note**: For the purpose of this problem, we define empty string as valid palindrome.

-**Example**
```
Input: "A man, a plan, a canal: Panama"
Output: true
Input: "race a car"
Output: false
```

#### Solution

Basically, it's a three step solution: turn to lowercase, check if a character is alphanumeric and then compare.
```
class Solution:
    def isPalindrome(self, s: str) -> bool:
        alpha = [chr(i) for i in range(97,123)]
        alpha += [chr(i) for i in range(48,58)]
        s = s.lower()
        clean = []
        for i in s:
            if i in alpha:
                clean.append(i)
        l = len(clean)
        if l <= 1:
            return True
        rm = l%2
        if rm == 1:
            left = clean[:l//2]
            right = clean[l//2+1:]
        else:
            left = clean[:l//2]
            right = clean[l//2:]
        right.reverse()
        for j in range(0,l//2):
            if left[j] != right[j]:
                return False
        return True
``` 
It can be also done by different order but with lower runtime.
```
class Solution:
    def isPalindrome(self, s: str) -> bool:
        l, r = 0, len(s)-1
        while l < r:
            while l < r and not s[l].isalnum():
                l += 1
            while l <r and not s[r].isalnum():
                r -= 1
            if s[l].lower() != s[r].lower():
                return False
            l +=1; r -= 1
        return True
```

# Problem No.08 String to Integer (atoi)

#### Problem Statement

Implement `atoi` which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.
-**Note**:
Only the space character ' ' is considered as whitespace character.
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2××31,  2××31 − 1]. If the numerical value is out of the range of representable values, **INT_MAX** (2××31 − 1) or **INT_MIN** (−2××31) is returned.

-**Example**
```
Input: "   -42"
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign.
             Then take as many numerical digits as possible, which gets 42.

Input: "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.

Input: "words and 987"
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical 
             digit or a +/- sign. Therefore no valid conversion could be performed.

Input: "-91283472332"
Output: -2147483648
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.
             Thefore INT_MIN (−231) is returned.
```

#### Solution

We can always do it in a dumb way.
```
class Solution:
    def myAtoi(self, str: str) -> int:
        if len(str) == 0:
            return 0
        nemuric_on = False
        plus_or_minus = False
        nemuric = [chr(i) for i in range(48,58)]
        for i in str:
            if i == ' ' and plus_or_minus == False and nemuric_on == False:
                result_str = []
            elif i == '+' and plus_or_minus == False and nemuric_on == False:
                result_str = []
                plus_or_minus = True
            elif i == '-' and plus_or_minus == False and nemuric_on == False:
                result_str = ["-"]
                plus_or_minus = True
            elif i in nemuric:
                if plus_or_minus == False and nemuric_on == False:
                    result_str = [i]
                else:
                    result_str.append(i)
                nemuric_on = True
            elif nemuric_on == True:
                break
            else:
                return 0
        result_str.append("0")
        result = int(''.join(result_str))//10
        if result < -2**31:
            result = -2**31
        elif result > 2**31-1:
            result = 2**31-1
        return result
```
Or we can use function `strip()` and a math way.
-**Note**
`strip()` can remove all the empty space in and just in the beginning and end.
`ord()` can turn back the ASCII or Unicode value. For example, ord("0") is 48. 
```
class Solution:
    def myAtoi(self, str: str) -> int:
        ls = list(str.strip())
        if len(ls) == 0 : return 0
        sign = -1 if ls[0] == '-' else 1
        if ls[0] in ['-','+'] : del ls[0]
        ret, i = 0, 0
        while i < len(ls) and ls[i].isdigit() :
            ret = ret*10 + ord(ls[i]) - ord('0')
            i += 1
        return max(-2**31, min(sign * ret,2**31-1))
```
