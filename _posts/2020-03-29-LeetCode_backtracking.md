---
layout:     post
title:      Solutions to Backtracking Problems in LeetCode(1)
subtitle:   LeetCode Problem No.17, No.22 and No.46
date:       2020-03-29
author:     JYW
header-img: img/post-200329-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.17 Letter Combinations of a Phone Number

#### Problem Statement

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![keypad](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

-**Example**：
```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

-**Note**:

Although the above answer is in lexicographical order, your answer could be in any order you want.

#### Solution

We can definitely list all combinations by adding each digit at one time.
```
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        res = []
        chs = [["a","b","c"],["d","e","f"],["g","h","i"],["j","k","l"],["m","n","o"],["p","q","r","s"],["t","u","v"],["w","x","y","z"]]
        
        def moreCombo(res:[], n: int):
            tmp = []
            if res == []:
                for letter in chs[n]:
                    tmp.append(letter)
            for each in res:
                for letter in chs[n]:
                    tmp.append(each+letter)
            return tmp
        
        for each in digits:
            res = moreCombo(res, int(each)-2)
            
        return res
``` 
Or we can use `dict` and solve it recursively.
```
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        phone = {'2': ['a', 'b', 'c'],
                 '3': ['d', 'e', 'f'],
                 '4': ['g', 'h', 'i'],
                 '5': ['j', 'k', 'l'],
                 '6': ['m', 'n', 'o'],
                 '7': ['p', 'q', 'r', 's'],
                 '8': ['t', 'u', 'v'],
                 '9': ['w', 'x', 'y', 'z']}    
        res = []
        
        def moreCombine(current, leftoverDigits):
            if not leftoverDigits:
                res.append(current)
                return 
            else:
                for each in phone[leftoverDigits[0]]:
                    moreCombine(current + each, leftoverDigits[1:])
        
        if not digits:
            return []
        else: 
            moreCombine("", digits)
            return res
```

# Problem No.22 Generate Parentheses

#### Problem Statement

Given *n* pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

-**Example**

For example, given n = 3, a solution set is:
```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

#### Solution

We can split the bracket into `"("` and `")"`. There are *n* brackets in total and we just need to decide when to open a bracket and when to close a bracket.
```
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        res = []
        
        def moreCombo(current = "", left = 0, right = 0):
            if len(current) == 2*n:
                res.append(current)
            else:
                if left < n:
                    moreCombo(current+"(", left+1, right)
                if right < left:
                    moreCombo(current+")", left, right+1)
        
        moreCombo()
        return res
``` 
However, we have another recursive solution which is quite amazing. It solves the problem by adding one bracket at one time to the left part and the right part remain the same.
```
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        if n == 0: 
            return ['']
        res = []
        for c in range(n):
            for left in self.generateParenthesis(c):
                for right in self.generateParenthesis(n-1-c):
                    res.append('({}){}'.format(left, right))
        return res
```

# Problem No.46 Permutations

#### Problem Statement

Given a collection of **distinct** integers, return all possible permutations.

-**Example**
```
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

#### Solution

We can solve the problem using brute force, by adding one element at one time.

```
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        if not nums:
            return []
        l = len(nums)
        res = [nums[0:1]]
        i = 1
        while i < l:
            next_res = []
            tmp = nums[i]
            for each in res:
                for j in range(len(each)):
                    next_res.append(each[:j]+[tmp]+each[j:])
                next_res.append(each+[tmp])
            res = next_res
            i += 1
        return res
```
Or we can do it recursively.
```
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        permutations = []
        if nums == []:
            return [[]]
        for idx, num in enumerate(nums):
            tmp_permutations = self.permute(nums[:idx]+nums[idx+1:])
            for permutation in tmp_permutations:
                permutation.insert(0,num)
            permutations.extend(tmp_permutations)
        return permutations
```

