---
layout:     post
title:      Tempt to Solve Piecewise Regression
subtitle:   Manually solve Piecewise Regression using Python.
date:       2020-02-28
author:     JYW
header-img: img/post-200228-algorithm1.jpg
catalog: true
tags:
    - Algorithm
    - Python
---

>I was asked by a professor to solve a Piecewise Regression problem. So here we go!

# Problem Statement

Write a function to perform single-variable linear Piecewise Regression. It sounds very simple: the minimum splits of samples are 3 because between two samples, the mean square error of linear model will always be 0. The ultimate goal is to find the best split combination with the minimum total mean square error.

# Solution

#### About Data

The data I use is a simple linear dataset with 15 samples. The data must be sorted by x variable.

```
import numpy as np
x = [3.4, 1.8, 4.6, 2.3, 3.1, 5.5, 0.7, 3.0, 2.6, 4.3, 2.1, 1.1, 6.1, 4.8,3.8]
y = [26.2, 17.8, 31.3, 23.1, 27.5, 36.5, 14.1, 22.3, 19.6, 31.3, 24.0, 17.3, 43.2, 36.4, 26.1]
dataset = np.dstack((x,y))
dataset = dataset[0]
d_arg = np.argsort(dataset[:,0])
dataset = dataset[d_arg]
```

#### Identify The Real Problem

The real problem in implementing this algorithm is finding all the possible combination of the splits. It's not like Apriori algorithms that can work from bottom to top. We have to list all combinations manually because there isn't a formula that guaranty the best split combination `f(n)` can be built on the result of a lower layer `f(n-1)`.

So I start with a function `f(n,m)` that can list all the split function when n samples are splited into m parts. The function starts with a bottom layer that each parts of the m parts have 3 elements, and everytime adding one element `f(n+1)`, the function will successively add this element into each combination from the result of last layer `f(n)`, until the last element added. Finally, the combinations with all elements will be logged into a list and the duplicate combinations will be removed.

```
def all_combination(n,m):
    result = []
    if n < 3*m:
        print('There are not enough elements to split.')
        return
    combination_bottom = [ 3 for n in range(m)]
    if n == 3*m:
        result.append(combination_bottom.copy())
    else:
        combination_now = [combination_bottom.copy()]
        for j in range(3*m+1,n+1):
            element_num = j
            combination_last = combination_now.copy()
            combination_now = []
            if element_num == n:
                for i in range (0, m):
                    for x in combination_last:
                        combination_new = x.copy()
                        combination_new[i] = combination_new[i]+1
                        combination_now.append(combination_new.copy())
                        if combination_new not in result:
                            result.append(combination_new.copy())
            else:
                for i in range (0, m):
                    for x in combination_last:
                        combination_new = x.copy()
                        combination_new[i] = combination_new[i]+1
                        combination_now.append(combination_new.copy())
    return result
``` 

#### Coding

The left part of coding is just calculate the mse value and the main function to run through all possible `m` value and compare the mse values to find the best one.

```
def calc_error(dataset):
    lr_model = linear_model.LinearRegression()
    x = pd.DataFrame(dataset[:,0])
    y = pd.DataFrame(dataset[:,1])
    lr_model.fit(x,y)
    predictions = lr_model.predict(x)
    mse = mean_squared_error(y, predictions)
    return mse

def calc_sum_error(dataset,cb):
    mse_sum = 0    
    for n in range(0,len(cb)):
        if n == 0:
            low = 0
            high = cb[n]
        else:
            low = 0
            for i in range(0,n):
                low += cb[i]
            high = low + cb[n]
        mse_sum += calc_error(dataset[low:high])
    return mse_sum

def best_piecewise(dataset):
    lenth = len(dataset)
    max_split = lenth // 3
    min_mse = calc_error(dataset)
    split_cb = []
    for i in range(2, max_split):
        split_result = all_combination(lenth, i)
        for cb in split_result:
            tmp_mse = calc_sum_error(dataset,cb)
            if tmp_mse < min_mse:
                min_mse = tmp_mse
                split_cb = cb
    return min_mse, split_cb
```

Finally we can plot the result.

```
min_mse, split_cb = best_piecewise(dataset)
plt.plot(x,y,"o")
for n in range(0,len(split_cb)):
    if n == 0:
        low = 0 
        high = split_cb[n]
    else:
        low = 0
        for i in range(0,n):
            low += split_cb[i]
        high = low + split_cb[n]
    x_tmp = pd.DataFrame(dataset[low:high,0])
    y_tmp = pd.DataFrame(dataset[low:high,1])
    lr_model = linear_model.LinearRegression()
    lr_model.fit(x_tmp,y_tmp)
    y_predict = lr_model.predict(x_tmp)
    plt.plot(x_tmp, y_predict, 'g-')

plt.show()
```
![Result](https://tva1.sinaimg.cn/large/00831rSTgy1gcdmlgermfj30ac070mx5.jpg)

#### Future Work

1. As we can see in the result, the linear models of different segmentations are not continuous. If continuous linear models are required, we can simply change the `calc_sum_error` function to achieve it.
2. The optimization of the functions can be improved such as remove duplicate results in the middle layer in `all_combination` function. Or we can think of another way that can solve the problem with lower complexity.
3. The Piecewise Regression model (aka, the segementation regression model) can also be based on a split point of the value of x variable (which can also generate a continuous model). Also, the minimum split number of 3 can be changed according to different problem and increase the generalization ability of the model.
