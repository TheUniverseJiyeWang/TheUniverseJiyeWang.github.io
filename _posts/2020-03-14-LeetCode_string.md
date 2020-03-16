---
layout:     post
title:      Solutions to String Problems in LeetCode(3)
subtitle:   LeetCode Problem #28, #38 and #14
date:       2020-03-14
author:     JYW
header-img: img/post-200314-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.28 Implement strStr()

#### Problem Statement

Implement `strStr()`.

Return the index of the first occurrence of needle in haystack, or **-1** if needle is not part of haystack.

-**Example**
```
Input: haystack = "hello", needle = "ll"
Output: 2
Input: haystack = "aaaaa", needle = "bba"
Output: -1
```
-**Clarification**:
What should we return when `needle` is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when needle is an empty string. This is consistent to C's `strstr()` and Java's `indexOf()`.

#### Solution

It's easy to develop an `O(m*n)` solution.
```
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        haystack = list(haystack)
        needle = list(needle)
        l_h = len(haystack)
        l_n = len(needle)
        if l_n == 0:
            return 0
        for i in range(0,l_h-l_n+1):
            if haystack[i] == needle[0]:
                j = 0
                while haystack[i+j] == needle[j]:
                    if j == l_n-1:
                        return i
                    j += 1
        return -1
``` 
Or we can directly compare two lists.
```
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        for i in range(len(haystack) - len(needle)+1):
            if haystack[i:i+len(needle)] == needle:
                return i
        return -1
```
Or we can first find all the possible matches and then check them with `O(m+n)` complexity.
```
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if haystack == None or needle == None:
            return -1
        i, j, m, n = -1, 0, len(haystack), len(needle)
        next = [-1] * n
        while j < n - 1:  
            if i == -1 or needle[i] == needle[j]:   
                i, j = i + 1, j + 1
                next[j] = i
            else:
                i = next[i]
        i = j = 0
        while i < m and j < n:
            if j == -1 or haystack[i] == needle[j]:
                i, j = i + 1, j + 1
            else:
                j = next[j]
        if j == n:
            return i - j
        return -1
```

# Problem No.38 Count and Say

#### Problem Statement

The count-and-say sequence is the sequence of integers with the first five terms as following:
```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```

`1` is read off as `"one 1"` or `11`.
`11` is read off as `"two 1s"` or `21`.
`21` is read off as `"one 2, then one 1"` or `1211`.

Given an integer n where 1 ≤ n ≤ 30, generate the nth term of the count-and-say sequence. You can do so recursively, in other words from the previous member read off the digits, counting the number of digits in groups of the same digit.

Note: Each term of the sequence of integers will be represented as a string.

-**Example**
```
Input: 1
Output: "1"
Explanation: This is the base case.

Input: 4
Output: "1211"
Explanation: For n = 3 the term was "21" in which we have two groups "2" and "1", "2" can be read as "12" which means frequency = 1 and value = 2, the same way "1" is read as "11", so the answer is the concatenation of "12" and "11" which is "1211".
```

#### Solution

Basically, we can count the continuous same numbers and then add two digits into the result string. The two digits represent the count nunber and the number counted. By recursively doing this for n-1 times, we can get the result string.
```
class Solution:
    def countAndSay(self, n: int) -> str:
        if n == 0:
            return ""
        result = "1"
        for i in range(0, n-1):
            current = result
            result = ""
            count = 1
            for j in range(1,len(current)+1):
                if j != len(current) and current[j] == current[j-1]:
                    count += 1
                else:
                    result = result + str(count) + current[j-1]
                    count = 1
        return result
``` 
Or we can use Regex to achieve it.
```
class Solution:
    def countAndSay(self, n: int) -> str:
        result = '1'
        for _ in range(n - 1):
            result = re.sub(r'(.)\1*', lambda m: str(len(m.group(0))) + m.group(1), result)
        return result
```
Or use the function `groupby()`
```
class Solution:
    def countAndSay(self, n: int) -> str:
        result = '1'
        for _ in range(n - 1):
            result = ''.join(str(len(list(group))) + digit for digit, group in itertools.groupby(result))
        return result
```

# Problem No.14 Longest Common Prefix

#### Problem Statement

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

-**Example**
```
Input: ["flower","flow","flight"]
Output: "fl"

Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

-**Note**:
All given inputs are in lowercase letters `a-z`.

#### Solution

I used the dataType `HashSet()` and checked its length to find the common prefix.
```
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        result = ""
        i = 0
        while True:
            tmp = []
            for string in strs:
                if i >= len(string):
                    return result
                tmp.append(string[i])
            if len(set(tmp)) != 1:
                break
            else:
                result = result + tmp[0]
            i += 1
        return result
```
Or we can do it each string by string.
```
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if not strs:
            return ""
        shortest = min(strs,key=len)
        for i, ch in enumerate(shortest):
            for other in strs:
                if other[i] != ch:
                    return shortest[:i]
        return shortest 
```