---
layout:     post
title:      Solutions to Backtracking Problems in LeetCode(2)
subtitle:   LeetCode Problem No.78 and No.79
date:       2020-04-02
author:     JYW
header-img: img/post-200402-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.78 Subsets

#### Problem Statement

Given a set of **distinct** integers, nums, return all possible subsets (the power set).

-**Note**:

The solution set must not contain duplicate subsets.

-**Example**：
```
Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

#### Solution

We can solve the problem recursively by traverse the nums list ont time and the result list for multiple times.
```
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        output = [[]]
        
        for num in nums:
            output += [curr + [num] for curr in output]
        
        return output
``` 
Or we can use backtracking. The time and space complexity is the same.
```
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        def backtrack(first = 0, curr = []):
            # if the combination is done
            if len(curr) == k:  
                output.append(curr[:])
            for i in range(first, n):
                # add nums[i] into the current combination
                curr.append(nums[i])
                # use next integers to complete the combination
                backtrack(i + 1, curr)
                # backtrack
                curr.pop()
        
        output = []
        n = len(nums)
        for k in range(n + 1):
            backtrack()
        return output
```

# Problem No.79 Word Search

#### Problem Statement

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

-**Example**

```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

-**Constraints**：

1. `board` and `word` consists only of lowercase and uppercase English letters.
2. `1 <= board.length <= 200`
3. `1 <= board[i].length <= 200`
4. `1 <= word.length <= 10^3`

#### Solution

We can define a backtracking function to check the nearby squares of a particular character and keep all existing suqares in a `dict`. If there is a posible way, then return True.
```
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        if word == "":
            return True
        if board == None or board == []:
            return False
        letters = len(word)
        rows = len(board)
        cols = len(board[0])
        if letters > rows * cols:
            return False
        neighbors = [(0,1),(1,0),(0,-1),(-1,0)]
        
        def dfs(board, word, x, y, streak, count):
            if count == letters:
                return True
            for(x1,y1) in neighbors:
                x2, y2 = x + x1, y + y1
                if 0 <= x2 < rows and 0 <= y2 < cols and (x2, y2) not in streak:
                    if board[x2][y2] == word[count]:
                        streak.add((x2,y2))
                        check = dfs(board, word, x2, y2, streak, count + 1)
                        if check:
                            return True
                        else:
                            streak.remove((x2, y2))
            return False
        
        for i in range(rows):
            for j in range(cols):
                if board[i][j] == word[0]:
                    streak = {(i, j)}
                    check = dfs(board, word, i, j, streak, 1)
                    if check:
                        return True
        return False
``` 
