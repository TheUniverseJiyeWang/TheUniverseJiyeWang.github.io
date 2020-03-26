---
layout:     post
title:      Solutions to Other Problems in LeetCode(2)
subtitle:   LeetCode Problem No.118, No.20 and No.268
date:       2020-03-25
author:     JYW
header-img: img/post-200325-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.118 Pascal's Triangle

#### Problem Statement

Given a non-negative integer numRows, generate the first numRows of Pascal's triangle.

![Pascal's Triangle](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

In Pascal's triangle, each number is the sum of the two numbers directly above it.

-**Example**：
```
Input: 5
Output:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

#### Solution

It's easy to come out with a loop solution.
```
class Solution:
    def generate(self, numRows: int) -> List[List[int]]:
        if numRows == 0:
            return []
        level = [1]
        result = [level]
        for i in range(1, numRows):
            last_level = level
            level = [0 for x in range(i+1)]
            level[0] = 1
            level[-1] = 1
            for j in range(0, i-1):
                level[j+1] = last_level[j]+last_level[j+1]
            result.append(level)
        return result
``` 
Or we can use `map()` function to achieve it.
```
class Solution:
    def generate(self, numRows: int) -> List[List[int]]:
        res = [[1]]
        for i in range(1, numRows):
            res.append(list(map(lambda x, y: x+y, res[-1] + [0], [0] + res[-1])))
        return res[:numRows]
```

# Problem No.20 Valid Parentheses

#### Problem Statement

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

-**Example**
```
Input: "()"
Output: true

Input: "()[]{}"
Output: true

Input: "(]"
Output: false

Input: "([)]"
Output: false

Input: "{[]}"
Output: true
```

#### Solution

I used the concept of stack to solve the problem.
```
class Solution:
    def isValid(self, s: str) -> bool:
        starts = ['(', '{', '[']
        ends = [')', '}', ']']
        tmp = []
        for ch in s:
            if ch in starts:
                tmp.append(ch)
            elif ch in ends:
                if tmp == []:
                    return False
                last_ch = tmp.pop()
                if ends.index(ch) != starts.index(last_ch):
                    return False
        if tmp != []:
            return False
        else:
            return True
``` 
We can also use `dict` instead of two lists.
```
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        mapping = {")": "(", "}": "{", "]": "["}
        for char in s:
            if char in mapping:
                top_element = stack.pop() if stack else '#'
                if mapping[char] != top_element:
                    return False
            else:
                stack.append(char)
        return not stack
```

# Problem No.268 Missing Number

#### Problem Statement

Given an array containing n distinct numbers taken from `0, 1, 2, ..., n`, find the one that is missing from the array.

-**Example**
```
Input: [3,0,1]
Output: 2

Input: [9,6,4,2,3,5,7,0,1]
Output: 8
```

-**Note**:
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

#### Solution

We can achieve it using simple math.

```
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        if not nums:
            return 0
        return int((len(nums)+1)*len(nums)/2-sum(nums))
```
Or we can use bit manipulation.
```
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        missing = len(nums)
        for i, num in enumerate(nums):
            missing ^= i ^ num
        return missing
```

