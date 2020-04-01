---
layout:     post
title:      Solutions to Other Problems in LeetCode(3)
subtitle:   LeetCode Problem No.371 and No.150
date:       2020-04-01
author:     JYW
header-img: img/post-200401-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.371 Pascal's Triangle

#### Problem Statement

Calculate the sum of two integers a and b, but you are not allowed to use the operator `+` and `-`.

-**Example**：
```
Input: a = 1, b = 2
Output: 3

Input: a = -2, b = 3
Output: 1
```

#### Solution

The first thought is using `sum()` HAHAHA!
```
class Solution:
    def getSum(self, a: int, b: int) -> int:
        return sum([a,b])
``` 
Or we can use bit manipulation.
```
class Solution:
    def adder(self,x,y):
        carry = 0
        while y:
            carry = x & y
            x ^= y
            y = carry << 1
        return x
    
    def subtractor(self,x,y):
        borrow = 0
        while y:
            borrow = (~x) & y
            x ^= y
            y = borrow << 1
        return x
    
    def getSum(self, a: int, b: int) -> int:
        a, b = max(a,b), min(a,b)
        if b >= 0:
            return self.adder(a,b)
        if a < 0:
            return -self.adder(-a,-b)
        if b < 0 and a >= 0:
            if -b > a:
                return -self.subtractor(-b, a)
            else:
                return self.subtractor(a, -b)
```

# Problem No.150 Evaluate Reverse Polish Notation

#### Problem Statement

Evaluate the value of an arithmetic expression in **Reverse Polish Notation**.

Valid operators are `+`, `-`, `*`, `/`. Each operand may be an integer or another expression.

-**Note**:

1. Division between two integers should truncate toward zero.
2. The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.

-**Example**
```
Input: ["2", "1", "+", "3", "*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9

Input: ["4", "13", "5", "/", "+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6

Input: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
Output: 22
Explanation: 
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

#### Solution

I used the concept of stack to solve the problem.
```
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        operators = ["+", "-", "*", "/"]
        numbers = []
        for each in tokens:
            if each not in operators:
                numbers.append(int(each))
            else:
                after = numbers.pop()
                before = numbers.pop()
                if each == "+":
                    numbers.append(before + after)
                elif each == "-":
                    numbers.append(before - after)
                elif each == "*":
                    numbers.append(before * after)
                else:
                    numbers.append(int(before / after))
        return numbers[0]
``` 
We can also use `dict` and `lambda`.
```
op = {'+': lambda x, y: x + y,
      '-': lambda x, y: x - y,
      '*': lambda x, y: x * y,
      '/': lambda x, y: int(x / y)}

class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        num_stack = []
        for t in tokens:
            if len(t) > 1 or t.isdigit():
                num_stack.append(int(t))
            else:
                b = num_stack.pop()
                a = num_stack.pop()
                num_stack.append(op[t](a, b))
        return num_stack[0]
```

