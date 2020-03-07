---
layout:     post
title:      Solutions to Array Nums Problems in LeetCode(2)
subtitle:   LeetCode Problem #189 and #217
date:       2020-03-06
author:     JYW
header-img: img/post-200229-LeetCode1.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.189 Rotate Array

#### Problem Statement

Given an array, rotate the array to the right by k steps, where k is non-negative.

-**Example**
```
Input: [1,2,3,4,5,6,7] and k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```

#### Solution

At first, I use a new **list** to store all changed numbers. However, the memory usage and runtime are both unsatisfying. I started to think if I can solve the problem not using a new list.
```
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        lenth = len(nums)
        real_k = k%lenth
        new = [0 for i in range(0, lenth)]
        for i in range(0, lenth):
            if i < real_k:
                new[i] = nums[i+lenth-real_k]
            else:
                new[i] = nums[i-real_k]
        for i in range(0, lenth):
            nums[i] = new[i]
``` 

This solution is quite interesting because it looks including a while loop inside a for loop at the first glance, but constrained by `count` number, it's actually of `O(n)` complexity. A tricky part of the solution is that: **Python Do Not Have** `Do While` **!!!**

![It follows the order like this](https://tva1.sinaimg.cn/large/00831rSTgy1gclt042lpqj30en0793yi.jpg)

```
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        lenth = len(nums)
        real_k = k%lenth
        count = 0
        if real_k == 0:
            return
        for i in range(0, lenth):
            if count < lenth:
                current = i
                prev = nums[i]
                while i != (current + real_k)%lenth:
                    if count < lenth:
                        new = (current + real_k)%lenth
                        tmp = nums[new]
                        nums[new] = prev
                        prev = tmp
                        current = new
                        count += 1
                else:
                	if count < lenth:
                    	new = (current + real_k)%lenth
                    	tmp = nums[new]
                    	nums[new] = prev
                    	prev = tmp
                    	current = new
                    	count += 1
```
However, we can do this in Python lol.

```
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        k %= len(nums)
        nums[k:], nums[:k] = nums[:-k], nums[-k:]
```

# Problem No.217 Contains Duplicate

#### Problem Statement

Given an array of integers, find if the array contains any duplicates.

Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

-**Example**
```
Input: [1,2,3,1]
Output: true
Input: [1,2,3,4]
Output: false
```

#### Solution

At first, I planned to use another list which remove all the duplicates and compare the length of two lists. If the lengths are different, the original list conatins duplicate. However, when I use dataType `list`, the runtime could be very high because everytime I checked with **"If `i` not in `rm_duplicates`"**, it's actually running a for loop.

```
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        rm_duplicate = []
        for i in nums:
            if i not in rm_duplicate:
                rm_duplicate.append(i)
        if len(rm_duplicate) == len(nums):
            return False
        else:
            return True
``` 

Then I chosed to use dataType `HashSet` which can reduce the runtime drastically. The time complexity is `O(n)`

```
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        rm_duplicate = set()
        for i in nums:
                rm_duplicate.add(i)
        if len(rm_duplicate) == len(nums):
            return False
        else:
            return True  
```

There is another solution where the `list` is **sorted** first and then each elements are compared with the next one to check if they are the same until finding the duplicates. The time complexity is `O(*n*log*n*)`

```
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        nums.sort()
        for i in range(0, len(nums)-1):
            if nums[i] == nums[i+1]:
                return True
        return False
```