---
layout:     post
title:      Solutions to Other Problems in LeetCode(2)
subtitle:   LeetCode Problem No.118, No.461 and No.190
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

-**Note**:

-Note that in some languages such as Java, there is no unsigned integer type. In this case, the input will be given as signed integer type and should not affect your implementation, as the internal binary representation of the integer is the same whether it is signed or unsigned.

-In Java, the compiler represents the signed integers using `2's complement notation`. Therefore, in Example 3 above the input represents the signed integer `-3`.

#### Solution

We can convert an int to base 3 and then count '1' in the converted string.
```
class Solution:
    def hammingWeight(self, n: int) -> int:
        n = bin(n)
        n_str = str(n)
        count = 0
        for ch in n_str:
            if ch == '1':
                count += 1
        return count
``` 
Or we can use bit operation.
```
class Solution:
    def hammingWeight(self, n: int) -> int:
        count = 0
        check = 1
        for i in range(0,32):
            if n&check != 0:
                count += 1
            check <<= 1
        return count
```
And more optimized one.
```
class Solution:
    def hammingWeight(self, n: int) -> int:
        count = 0
        while n != 0:
            count += 1
            n &= (n-1)
        return count
```

# Problem No.461 Hamming Distance

#### Problem Statement

The Hamming distance between two integers is the number of positions at which the corresponding bits are different.

Given two integers `x` and `y`, calculate the Hamming distance.

-**Note**:

0 ≤ `x`, `y` < 231.

-**Example**
```
Input: x = 1, y = 4

Output: 2

Explanation:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑

The above arrows point to positions where the corresponding bits are different.
```

#### Solution

We can easily solve the problem by XOR.
```
class Solution:
    def hammingDistance(self, x: int, y: int) -> int:
        return bin(x^y).count('1')
``` 

# Problem No.190 Reverse Bits

#### Problem Statement

Reverse bits of a given 32 bits unsigned integer.

-**Example**
```
Input: 00000010100101000001111010011100
Output: 00111001011110000010100101000000
Explanation: The input binary string 00000010100101000001111010011100 represents the unsigned integer 43261596, so return 964176192 which its binary representation is 00111001011110000010100101000000.

Input: 11111111111111111111111111111101
Output: 10111111111111111111111111111111
Explanation: The input binary string 11111111111111111111111111111101 represents the unsigned integer 4294967293, so return 3221225471 which its binary representation is 10111111111111111111111111111111.
```

#### Solution

We can use `format()` function and reverse the string to solve the problem.

```
class Solution:
    def reverseBits(self, n: int) -> int:
        oribin='{0:032b}'.format(n)
        reversebin=oribin[::-1]
        return int(reversebin,2)
```
Or we can use bit manipulation.
```
class Solution:
    def reverseBits(self, n: int) -> int:
        ans = 0
        for i in range(32):
            ans = (ans << 1) + (n & 1)
            n >>= 1
        return ans
```

