---
layout:     post
title:      Solutions to Sort and Search Problems in LeetCode(2)
subtitle:   LeetCode Problem No.75, No.374 and No.215
date:       2020-03-27
author:     JYW
header-img: img/post-200327-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.88 Sort Colors

#### Problem Statement

Given an array with n objects colored red, white or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

-**Note**:

 You are not suppose to use the library's sort function for this problem.

-**Example**
```
Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```
-**Follow up**:

A rather straight forward solution is a two-pass algorithm using counting sort.
First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
Could you come up with a one-pass algorithm using only constant space?

#### Solution

It can definitely solved by `sort()` function. However, since there are only three numbers in the lists, we can optimize the solution by pointers. If we find any number other than 2, then we can changed the elements from the beginning.
```
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        i = j = 0
        for k in range(len(nums)):
            v = nums[k]
            nums[k] = 2
            if v < 2:
                nums[j] = 1
                j += 1
            if v == 0:
                nums[i] = 0
                i += 1
``` 
Or we can use another pointer backwards, which can be more efficient.
```
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        red, white, blue = 0, 0, len(nums)-1
    
        while white <= blue:
            if nums[white] == 0:
                nums[red], nums[white] = nums[white], nums[red]
                white += 1
                red += 1
            elif nums[white] == 1:
                white += 1
            else:
                nums[white], nums[blue] = nums[blue], nums[white]
                blue -= 1
```

# Problem No.374 Top K Frequent Elements

#### Problem Statement

Given a non-empty array of integers, return the k most frequent elements.

-**Example**

```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]

Input: nums = [1], k = 1
Output: [1]
```

-**Note**:

1. You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
2. Your algorithm's time complexity must be better than O(n log n), where n is the array's size.

#### Solution

We can use `count()` and `sort()` to solve the problem directly. However, it's not efficient enough.
```
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = {}
        for each in nums:
            if each not in count:
                count[each] = 1
            else:
                count[each] += 1
        count = sorted(count.items(), key=lambda d: d[1], reverse=True)
        res = []
        c = 0
        for el in count:
            if c == k:
                break
            else:
                res.append(el[0])
                c += 1
        return res
``` 
We can also use `heap()` function to find the first k largest numbers.
```
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = collections.Counter(nums)   
        return heapq.nlargest(k, count.keys(), key=count.get)
```

# Problem No.215 Kth Largest Element in an Array

#### Problem Statement

Find the **k**th largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

-**Example**

```
Input: [3,2,1,5,6,4] and k = 2
Output: 5

Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
```

-**Note**:

You may assume k is always valid, 1 ≤ k ≤ array's length.

#### Solution

Yes, we can definitely use `sort()`.
```
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        nums.sort()
        return nums[-k]
```
Or we can use quick select.
```
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        pivot = nums[0]
        left = []
        equal = []
        right = []
        for each in nums:
            if each < pivot:
                left.append(each)
            elif each == pivot:
                equal.append(each)
            else:
                right.append(each)

        if k <= len(right):
            return self.findKthLargest(right, k)
        elif (k - len(right)) <= len(equal):
            return equal[0]
        else:
            return self.findKthLargest(left, k - len(right) - len(equal))
```
We can also use a reversed quick sort and stop the sorting porcess when the **k**th element is already sorted.
```
class Solution:
    def findKthLargest(self, nums: [int], k: int) -> int:
        
        def quickSort(array:[int], left: int, right: int, k:int):
            if left >= right:
                return
            low = left
            high = right
            key = array[high]
            l = len(array)
            while left < right:
                while left < right and array[left] < key:
                    left += 1
                array[right] = array[left]
                while left < right and array[right] >= key:
                    right -= 1
                array[left] = array[right]
            array[left] = key
            distance = l-left
            if distance == k:
                return
            else:
                quickSort(array, left + 1, high, k)
                quickSort(array, low, left - 1, 0)
            
        quickSort(nums,0,len(nums)-1,k)
        return nums[-k]
```