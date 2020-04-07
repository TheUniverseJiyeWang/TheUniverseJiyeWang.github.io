---
layout:     post
title:      Solutions to Array Nums Problems in LeetCode(10=6+3+1)
subtitle:   LeetCode Problem No.169, No.139 and No.140
date:       2020-04-06
author:     JYW
header-img: img/post-200406-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.169 Majority Element

#### Problem Statement

Given an array of size *n*, find the majority element. The majority element is the element that appears **more than** `⌊ n/2 ⌋` times.

You may assume that the array is non-empty and the majority element always exist in the array.

-**Note**:

The solution set must not contain duplicate triplets.

-**Example**
```
Input: [3,2,3]
Output: 3

Input: [2,2,1,1,1,2,2]
Output: 2
```

#### Solution
We can definitely sort the list and then return the middle element which is exact the majority element. 
```
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        nums.sort()
        return nums[len(nums)//2]
``` 

But we can still optmize the solution using Boyer-Moore Voting Algorithm to reduce the time complexity from `O(nlog(n))` to `O(n)`. There is only two condition that the current number is or is not the majority elements. We can use int `candidate` to switch to different numbers until another int `count` stay positive and that number is the majority element.
```
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        count = 0
        candidate = None

        for num in nums:
            if count == 0:
                candidate = num
            count += (1 if num == candidate else -1)

        return candidate
```

# Problem No.139 Word Break

#### Problem Statement

Given a **non-empty** string s and a dictionary *wordDict* containing a list of *non-empty* words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

-**Note**:

1. The same word in the dictionary may be reused multiple times in the segmentation.
2. You may assume the dictionary does not contain duplicate words.

-**Example**
```
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".

Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.

Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

#### Solution

In python we can import Queue and use `startswith` to check if the string start with any words in the dict. If there are multiple words that can fits into the `startswith()` function, we can use a `set()` to keep track on the branches until there is no possible combinations.
```
from queue import Queue
class Solution:
    def bfs(self,node,wordDict,s,visited):
        if  node == s:
            return True
        q = Queue()
        q.put(node)
        while q.empty()==False:     
            curr = q.get() 
            if curr == s:
                return True
            for next_word in wordDict:
                l = len(curr)
                if s[l:].startswith(next_word) and len(curr+next_word) not in visited:
                    q.put(curr+next_word)
                    visited.add(len(curr+next_word))
        return False
                
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        visited = set()
        for words_from in wordDict:
            if s.startswith(words_from) and len(words_from) not in visited:
                if self.bfs(words_from,wordDict,s,visited):
                    return True
        return False
``` 

# Problem No.140 Word Break II

#### Problem Statement

Given a **non-empty** string s and a dictionary wordDict containing a list of **non-empty** words, add spaces in s to construct a sentence where each word is a valid dictionary word. Return all such possible sentences.

-**Note**:

1. The same word in the dictionary may be reused multiple times in the segmentation.
2. You may assume the dictionary does not contain duplicate words.

-**Example**
```
Input:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
Output:
[
  "cats and dog",
  "cat sand dog"
]

Input:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
Output:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
Explanation: Note that you are allowed to reuse a dictionary word.

Input:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
Output:
[]
```

#### Solution

We can do it in a dfs way.
```
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> List[str]:
        wordSet =set(wordDict)
        @functools.lru_cache
        def dfs(start: int):
            if start == len(s):
                return [[]]
            res = [] # lru_cache doesn't work with generator. has to collect results manually
            for i in range(start, len(s)):
                if s[start:i+1] in wordSet:
                    res += [[s[start:i+1]] + child for child in dfs(i+1)]
            return res
        return [" ".join(line) for line in dfs(0)]
```
