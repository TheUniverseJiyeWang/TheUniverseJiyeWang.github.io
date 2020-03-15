---
layout:     post
title:      Solutions to Array Nums Problems in LeetCode(5)
subtitle:   LeetCode Problem No.36 and No.48
date:       2020-03-13
author:     JYW
header-img: img/post-200313-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.36 Valid Sudoku

#### Problem Statement

Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

-1. Each row must contain the digits `1-9` without repetition.
-2. Each column must contain the digits `1-9` without repetition.
-3. Each of the 9 `3x3` sub-boxes of the grid must contain the digits 1-9 without repetition.

The Sudoku board could be partially filled, where empty cells are filled with the character `'.'`.

-**Example**
```
Input:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: true

Input:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being 
    modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```
-**Note**:
A Sudoku board (partially filled) could be valid but is not necessarily solvable.
Only the filled cells need to be validated according to the mentioned rules.
The given board contain only digits `1-9` and the character `'.'`.
The given board size is always `9x9`.

#### Solution

The problem can be solved by manually checking each line, column and square.
```
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        digits = [chr(i) for i in range(48,58)]
        for i in range(0,9):
            horizontal = []
            vertical = []
            for j in range(0,9):
                if board[i][j] in digits:
                    if board[i][j] not in horizontal:
                        horizontal.append(board[i][j])
                    else:
                        return False
                if board[j][i] in digits:
                    if board[j][i] not in vertical:
                        vertical.append(board[j][i])
                    else:
                        return False
        h_total = v_total= [[0,1,2],[3,4,5],[6,7,8]]
        for h_sec in h_total:
            for v_sec in v_total:
                square = []
                for i in h_sec:
                    for j in v_sec:
                        if board[i][j] in digits:
                            if board[i][j] not in square:
                                square.append(board[i][j])
                            else:
                                return False
        return True
``` 
We can also use dataType `HashSet()` and compare the length.
```
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        return (self.is_row_valid(board) and
                self.is_col_valid(board) and
                self.is_square_valid(board))

    def is_row_valid(self, board):
        for row in board:
            if not self.is_unit_valid(row):
                return False
        return True

    def is_col_valid(self, board):
        for col in zip(*board):
            if not self.is_unit_valid(col):
                return False
        return True
    
    def is_square_valid(self, board):
        for i in (0, 3, 6):
            for j in (0, 3, 6):
                square = [board[x][y] for x in range(i, i + 3) for y in range(j, j + 3)]
                if not self.is_unit_valid(square):
                    return False
        return True
        
    def is_unit_valid(self, unit):
        unit = [i for i in unit if i != '.']
        return len(set(unit)) == len(unit)
```
Or we can use bit operation.
```
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        vset = [0 for i in range(0,9)]
        hset = [0 for i in range(0,9)]
        sqset = [0 for i in range(0,9)]
        idx = 0
        for i in range(0,9):
            for j in range(0,9):
                if board[i][j] != '.':
                    idx = 1 << (ord(board[i][j])-ord('0'))
                    if (hset[i] & idx) > 0 or (vset[j] & idx) > 0 or (sqset[(i//3)*3+(j//3)] & idx) >0:
                        return False
                    hset[i] |= idx
                    vset[j] |= idx
                    sqset[(i//3)*3+(j//3)] |= idx
        return True
```

# Problem No.48 Rotate Image

#### Problem Statement

You are given an n x n 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

-**Note**:
You have to rotate the image **in-place**, which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

-**Example**
```
Given input matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

rotate the input matrix **in-place** such that it becomes:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

#### Solution

The tricky part of the problem is to figure out the rule of changed position. Then I just have to move 1/4 of all elements for a fixed four times.
```
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        l = len(matrix)
        if l%2 == 0:
            h = v = l//2
        else:
            h = l//2+1
            v = l//2
        for i in range(0, h):
            for j in range(0, v):
                prev = matrix[i][j]
                current = matrix[j][l-i-1]
                matrix[j][l-i-1] = prev
                prev = current
                current = matrix[l-i-1][l-j-1]
                matrix[l-i-1][l-j-1] =  prev
                prev = current
                current = matrix[l-j-1][i]
                matrix[l-j-1][i] = prev
                prev = current
                matrix[i][j] = prev
``` 
