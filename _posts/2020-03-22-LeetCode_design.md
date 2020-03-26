---
layout:     post
title:      Solutions to Design Problems in LeetCode(1)
subtitle:   LeetCode Problem No.384 and No.155
date:       2020-03-22
author:     JYW
header-img: img/post-200322-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.384 Shuffle an Array

#### Problem Statement

You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

-**Note**:

Shuffle a set of numbers without duplicates.

-**Example**：
```
// Init an array with set 1, 2, and 3.
int[] nums = {1,2,3};
Solution solution = new Solution(nums);

// Shuffle the array [1,2,3] and return its result. Any permutation of [1,2,3] must equally likely to be returned.
solution.shuffle();

// Resets the array back to its original configuration [1,2,3].
solution.reset();

// Returns the random shuffling of array [1,2,3].
solution.shuffle();
```

#### Solution

I just used function `random()` to find a randomly changed index and then use a for loop to swap with random index.
```
class Solution:

    def __init__(self, nums: List[int]):
        self.array = nums
        self.origin = list(nums)
        

    def reset(self) -> List[int]:
        """
        Resets the array to its original configuration and return it.
        """
        self.array = self.origin
        self.origin = list(self.origin)
        return self.array

    def shuffle(self) -> List[int]:
        """
        Returns a random shuffling of the array.
        """
        for i in range(len(self.array)):
            idx = random.randrange(i,len(self.array))
            self.array[i], self.array[idx] = self.array[idx], self.array[i]
        return self.array
``` 
Or we can use `lambda` to achieve it.
```
class Solution:

    def __init__(self, nums: List[int]):
        self.reset = lambda: nums
        self.shuffle = lambda: random.sample(nums, len(nums))
```

# Problem No.155 Min Stack

#### Problem Statement

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

-push(x) -- Push element x onto stack.
-pop() -- Removes the element on top of the stack.
-top() -- Get the top element.
-getMin() -- Retrieve the minimum element in the stack.

-**Example**
```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.
```

#### Solution

We can definately achieve it in Python using `list`.
```
class MinStack:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.list = []

    def push(self, x: int) -> None:
        self.list.append(x)

    def pop(self) -> None:
        if len(self.list) > 0:
            self.list.pop()

    def top(self) -> int:
        if len(self.list) > 0:
            return self.list[-1]
        else:
            return None

    def getMin(self) -> int:
        if len(self.list) > 0:
            return min(self.list)
        else:
            return None
``` 
Or we can use a list of `tuple` to drastically reduce the runtime.
```
class MinStack:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.list = []

    def push(self, x: int) -> None:
        curMin = self.getMin()
        if curMin == None or x < curMin:
            curMin = x
        self.list.append((x, curMin));

    def pop(self) -> None:
        self.list.pop()

    def top(self) -> int:
        if len(self.list) > 0:
            return self.list[-1][0]
        else:
            return None

    def getMin(self) -> int:
        if len(self.list) > 0:
            return self.list[-1][1]
        else:
            return None
```
