---
layout:     post
title:      Solutions to String Problems in LeetCode(4)
subtitle:   LeetCode Problem No.49, No.03 and No.05
date:       2020-03-14
author:     JYW
header-img: img/post-200402-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.49 Group Anagrams

#### Problem Statement

Given an array of strings, group anagrams together.

-**Example**
```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```
-**Note**:

1. All inputs will be in lowercase.
2. The order of your output does not matter.

#### Solution

We can definitely use a `dict` to storage and check unique anagrams by sorted string.
```
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        i = 0
        dic = {}
        res = []
        for each in strs:
            ana = list(each)
            ana.sort()
            ana = "".join(ana)
            if ana not in dic:
                dic[ana] = i
                res.append([each])
                i += 1
            else:
                res[dic[ana]].append(each)
        return res
``` 
Or we can use count numbers to check anagrams.
```
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        ans = collections.defaultdict(list)
        for s in strs:
            count = [0] * 26
            for c in s:
                count[ord(c) - ord('a')] += 1
            ans[tuple(count)].append(s)
        return ans.values()
```

# Problem No.3 Longest Substring Without Repeating Characters

#### Problem Statement

Given a string, find the length of the **longest substring** without repeating characters.

-**Example**
```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 

Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.

Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

#### Solution

We can go through the list of each character and find each substring without repeating and then find the longest one.
```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        str_list = list(s)
        max_length = 0
        tmp = []
        for ch in str_list:
            if ch not in tmp:
                tmp.append(ch)
            else:
                if len(tmp) > max_length:
                    max_length = len(tmp)
                idx = tmp.index(ch)
                tmp = tmp[idx+1:]
                tmp.append(ch)
        if len(tmp) > max_length:
            max_length = len(tmp)
        return max_length
``` 
Or we can count the distance between same charactres.
```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        l = len(s)
        res = 0
        index = [0 for x in range(128)]
        i = 0
        j = 0
        while j < l:
            i = max(index[ord(s[j])], i)
            res = max(res, j-i+1)
            index[ord(s[j])] = j + 1
            j += 1
        return res
```

# Problem No.05 Longest Palindromic Substring

#### Problem Statement

Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.

-**Example**
```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.

Input: "cbbd"
Output: "bb"
```

#### Solution

This is a `O(n)` time complexity solution. Expand function return a max length of a palindromic substring from particular position and then we need to traverse the string and find the longest one.
```
class Solution:
    def expand(self, s: str, left: int, right: int) -> int:
        l, r = left, right
        while l >= 0 and r < len(s) and s[l] == s[r]:
            l -= 1
            r += 1
        return r-l-1
    
    def longestPalindrome(self, s: str) -> str:
        if s == None or len(s) < 1:
            return ""
        start = 0
        end = 0
        for i in range(len(s)):
            len1 = self.expand(s,i,i)
            len2 = self.expand(s,i,i+1)
            len_tmp = max(len1,len2)
            if len_tmp > end - start:
                start = i - (len_tmp-1)//2
                end = i + len_tmp//2
        return s[start: end+1]
```
