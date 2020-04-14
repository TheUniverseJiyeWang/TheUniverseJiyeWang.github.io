---
layout:     post
title:      Solutions to Design Problems in LeetCode(2)
subtitle:   LeetCode Problem No.297, No.380 and No.146
date:       2020-04-10
author:     JYW
header-img: img/post-200410-LeetCode1-01.jpg
catalog: true
tags:
    - LeetCode
    - Python
    - Algorithm
---

>Learn a bit of algorithms everyday!

# Problem No.297 Serialize and Deserialize Binary Tree

#### Problem Statement

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

-**Example**：
```
You may serialize the following tree:

    1
   / \
  2   3
     / \
    4   5

as "[1,2,3,null,null,4,5]"
```

-**Clarification**:

The above format is the same as how LeetCode serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

-**Note**:

Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

#### Solution

By using level order traverse, we can keep track on each node and append related string to the result list. And by reversing the process, we can generate a binary tree using a list of string.
```
class Codec:
    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        if not root:
            return "#"
        queue = [root]
        res = [str(root.val)]
        while queue:
            res += [str(node.val) if node else "#" for root in queue for node in (root.left,root.right)]
            queue = [node for root in queue for node in (root.left, root.right) if node]
        return ",".join(res)

    def deserialize(self, data):
        if data == "#":
            return None
        d = iter(data.split(","))
        root = TreeNode(int(next(d)))
        queue = [root]
        while queue:
            for node in queue:
                left = next(d)
                node.left = TreeNode(int(left)) if left!="#" else None
                right = next(d)
                node.right = TreeNode(int(right)) if right!="#" else None
            queue = [node for root in queue for node in (root.left, root.right) if node]
        return root
``` 

# Problem No.380 Insert Delete GetRandom O(1)

#### Problem Statement

Design a data structure that supports all following operations in average O(1) time.

1. `insert(val)`: Inserts an item val to the set if not already present.
2. `remove(val)`: Removes an item val from the set if present.
3. `getRandom`: Returns a random element from current set of elements. Each element must have the same probability of being returned.

-**Example**
```
// Init an empty set.
RandomizedSet randomSet = new RandomizedSet();

// Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomSet.insert(1);

// Returns false as 2 does not exist in the set.
randomSet.remove(2);

// Inserts 2 to the set, returns true. Set now contains [1,2].
randomSet.insert(2);

// getRandom should return either 1 or 2 randomly.
randomSet.getRandom();

// Removes 1 from the set, returns true. Set now contains [2].
randomSet.remove(1);

// 2 was already in the set, so return false.
randomSet.insert(2);

// Since 2 is the only number in the set, getRandom always return 2.
randomSet.getRandom();
```

#### Solution

We can use `set()` to solve the problem.
```
class RandomizedSet:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.s = set()

    def insert(self, val: int) -> bool:
        """
        Inserts a value to the set. Returns true if the set did not already contain the specified element.
        """
        if val not in self.s:   
            self.s.add(val)
            return True
        

    def remove(self, val: int) -> bool:
        """
        Removes a value from the set. Returns true if the set contained the specified element.
        """
        if val in self.s:
            self.s.remove(val)
            return True

    def getRandom(self) -> int:
        """
        Get a random element from the set.
        """
        return list(self.s)[random.randint(0, len(self.s) - 1)]
``` 

# Problem No.146 LRU Cache

#### Problem Statement

Design and implement a data structure for `Least Recently Used (LRU) cache`. It should support the following operations: get and put.

`get(key)` - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
`put(key, value)` - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

The cache is initialized with a **positive** capacity.

-**Follow up**:

Could you do both operations in `O(1)` time complexity?

-**Example**
```
LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

#### Solution

We can use a `dict()` to store the keys and values and a counter to store which one is the least recently visited key.
```
class LRUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.counter = 0
        self.values = dict()   # Key: [value, HowRecentAccessed]
        
    def get(self, key: int) -> int:
        
        if key in self.values.keys():
            val = self.values[key]
            val[1] = self.counter+1
            self.counter +=1
            self.values[key] = val
            return val[0]
        else:
            return -1
        

    def put(self, key: int, value: int) -> None:
        
        if len(self.values) < self.capacity or key in self.values.keys():
            val = [value, self.counter]
            self.counter +=1
            self.values[key] = val
        else:
            mn = min(self.values.items(), key = lambda x: x[1][1])[0]
            del self.values[mn]
            val = [value, self.counter]
            self.counter +=1
            self.values[key] = val
```