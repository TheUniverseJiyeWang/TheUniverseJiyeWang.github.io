---
layout:     post
title:      Solutions to Math Problems in LeetCode(5)
subtitle:   LeetCode Problem No.29, No.166, No.179 and No.149
date:       2020-04-09
author:     JYW
header-img: img/post-200409-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.29 Divide Two Integers

#### Problem Statement

Given two integers `dividend` and `divisor`, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing `dividend` by `divisor`.

The integer division should truncate toward zero, which means losing its fractional part. For example, `truncate(8.345) = 8` and `truncate(-2.7335) = -2`.

-**Example**：
```
Input: dividend = 10, divisor = 3
Output: 3
Explanation: 10/3 = truncate(3.33333..) = 3.

Input: dividend = 7, divisor = -3
Output: -2
Explanation: 7/-3 = truncate(-2.33333..) = -2.
```

-**Note**:

1. Both dividend and divisor will be 32-bit signed integers.
2. The divisor will never be 0.
3. Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function **returns 231 − 1 when the division result overflows**.

#### Solution

If we use loop to calculate the result, the run time could be extremly high.
```
class Solution:
    def divide(self, dividend: int, divisor: int) -> int:
        if dividend == 0:
            return 0
        a = 1
        if dividend > 0 and divisor < 0:
            a = -1
        if dividend < 0 and divisor > 0:
            a = -1
        dividend = abs(dividend)
        divisor = abs(divisor)
        count = 0
        res = 0
        while res <= dividend:
            res += divisor
            count += 1
        return (count - 1)*a
```
But we can apply binary search to reduce the time complexity.
```
class Solution:
    def divide(self, dividend: int, divisor: int) -> int:
        negative = False
        if (dividend < 0 and divisor > 0) or (dividend > 0 and divisor < 0):
            negative = True
        ans, count, abDividend, abDivisor = 0, 1, abs(dividend), abs(divisor)
        while abDividend >= abDivisor:
            abDivisor = abDivisor << 1
            
            if abDividend >= abDivisor:
                count = count << 1
            
            else:
                abDivisor = abDivisor >> 1
                abDividend = abDividend - abDivisor
                abDivisor = abs(divisor)
                ans += count
                count = 1
        if negative:
            ans = -ans
        if ans > 2147483647:
            return 2147483647
        return ans
```

# Problem No.166 Fraction to Recurring Decimal

#### Problem Statement

Given two integers representing the numerator and denominator of a fraction, return the fraction in string format.

If the fractional part is repeating, enclose the repeating part in parentheses.

-**Example**:
```
Input: numerator = 1, denominator = 2
Output: "0.5"

Input: numerator = 2, denominator = 1
Output: "2"

Input: numerator = 2, denominator = 3
Output: "0.(6)"
```

#### Solution

Since it's a fraction, the dividend and the divisor are both int. In this condition, we can find the loop by check if there is a same number appear in the decimal part if there is a loop.
```
class Solution:
    def fractionToDecimal(self, numerator: int, denominator: int) -> str:
        
        if(numerator == 0): 
            return '0'
        
        fraction = list()
        # check postive or negative
        if((numerator < 0 and denominator > 0) or (numerator > 0 and denominator < 0)):
            fraction.append('-') 
            
        # the positive or negative sign is already been taken care of, just use the absolute value to avoid integer division problems
        numerator = abs(numerator)
        denominator = abs(denominator)

        
        fraction.append(str(numerator // denominator))
        # update remainder
        remainder = numerator % denominator
        
        # no fractions
        if(remainder == 0):
            return ''.join(fraction)
        
        # add deciable point
        fraction.append('.')
        
        numDict = dict()
        # run the division until no remainder
        while(remainder != 0):
            # if there exist a loop
            if(remainder in numDict):
                # add a left parenthese at the beginning of loop
                fraction.insert(numDict[remainder], '(')
                fraction.append(')')
                break
                
            # add this part of remainder into map, mark as seen
            numDict[remainder] = len(fraction)
            # make remainder integers
            remainder *= 10
            this_fraction = remainder // denominator
            fraction.append(str(this_fraction))
            
            # update remainder, as new remainder
            remainder %= denominator;

        
        return ''.join(fraction)
``` 

# Problem No.179 Largest Number

#### Problem Statement

Given a list of non negative integers, arrange them such that they form the largest number.

-**Example**:
```
Input: [10,2]
Output: "210"

Input: [3,30,34,5,9]
Output: "9534330"
```

#### Solution

We can define a key to compare by which orders it creates a larger number and use this key to sort the list.
```
class LargerNumKey(str):
    def __lt__(x, y):
        return x+y > y+x
        
class Solution:
    def largestNumber(self, nums):
        largest_num = ''.join(sorted(map(str, nums), key=LargerNumKey))
        return '0' if largest_num[0] == '0' else largest_num
```

# Problem No.149 Max Points on a Line

#### Problem Statement

Given *n* points on a 2D plane, find the maximum number of points that lie on the same straight line.

-**Example**:
```
Input: [[1,1],[2,2],[3,3]]
Output: 3
Explanation:
^
|
|        o
|     o
|  o  
+------------->
0  1  2  3  4

Input: [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]
Output: 4
Explanation:
^
|
|  o
|     o        o
|        o
|  o        o
+------------------->
0  1  2  3  4  5  6
```

#### Solution

At first, I tried to keep track on distinct lines and check those lines. However, the calculation to find lines may take extremly long time.
```
from fractions import Fraction
class Solution:
    def maxPoints(self, points: List[List[int]]) -> int:
        if len(points) < 2:
            return len(points)
        ab = []
        maxpoints = 0
        for i in range(0,len(points)):
            for j in range(1, len(points)):
                if i != j:
                    if points[j][0]-points[i][0] == 0:
                        a = points[j][0]
                        b = None
                        if (a,b) not in ab:
                            ab.append((a,b))
                            count = 0
                            for point in points:
                                if point[0] == a:
                                    count += 1
                    else:
                        a = Fraction((points[j][1]-points[i][1]),(points[j][0]-points[i][0]))
                        b = points[i][1] - a*points[i][0]
                        if (a,b) not in ab:
                            ab.append((a,b))
                            count = 0
                            for point in points:
                                if a*point[0]+b == point[1]:
                                    count += 1
                    if count >= maxpoints:
                        maxpoints = count
        return maxpoints
```
The best solution is to use gcd to keep track on angles. But it's quite the same using fraction or gcd. The difference is how we keep track on lines and keep minimum traverse.
```
class Solution:
    def gcd(self,a,b):
        if b==0:
            return a
        else:
            return self.gcd(b,a%b)
        
    def maxPoints(self, points: List[List[int]]) -> int:
        if len(points)<3:
            return len(points)
        
        maxpoints=0
        
        for i in range(len(points)-1):
            
            table={90:1}
            same=0
            count=1
            for j in range(i+1,len(points)):
                
                if points[i][0]==points[j][0] and points[i][1]==points[j][1]:
                    same+=1
                    continue
                elif points[i][0]==points[j][0]:
                    angle=90
                else:
                    gcdofpoints=self.gcd(points[i][1]-points[j][1],points[i][0]-points[j][0])
                    angle=((points[i][1]-points[j][1])//gcdofpoints,(points[i][0]-points[j][0])//gcdofpoints)
                if angle not in table:
                    table[angle]=1
                table[angle]+=1
                count=max(count,table[angle])
            maxpoints=max(maxpoints,count+same)
        
        return maxpoints
```
I changed my solution to the same traverse method and it also worked. However, the `Fraction()` takes much more time the gcd.
```
from fractions import Fraction
class Solution:
    def maxPoints(self, points: List[List[int]]) -> int:
        if len(points) < 2:
            return len(points)
        maxpoints = 0
        for i in range(0,len(points)-1):
            ab = {90:1}
            same = 0
            count = 1
            for j in range(i+1, len(points)):
                if points[i][0]==points[j][0] and points[i][1]==points[j][1]:
                    same+=1
                    continue
                elif points[i][0]==points[j][0]:
                    angle=90
                else:
                    angle = Fraction((points[j][1]-points[i][1]),(points[j][0]-points[i][0]))
                    
                if angle not in ab:
                    ab[angle] = 1
                ab[angle] += 1
                count = max(count, ab[angle])
            maxpoints = max(maxpoints,count+same)
        return maxpoints
```
