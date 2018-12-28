---
layout: post
comments: true
title: "Pandas - Quick Start"
date: "2018-12-28 10:27:00"
category: [Python]
tag: [pandas, python]
---

> [*pandas*](https://pandas.pydata.org/index.html) is an open source,  BSD-licensed library providing high-performance, easy-to-use data structures and data analysis tools for the **Python** programming language.

<!--more-->

## 1. Overview - Data Structure

---|---|---
**Dimensions** | **Name** | **Description**
1          | Series | 1D labeled homogeneously-typed array
2          | DataFrame | General 2D labeled, size-mutable tabular structure with potentially heterogeneously-typed column

> **Why more than one data structure?**
>
> The best way to think about the pandas data structures is as flexible containers for lower dimensional data. For example, DataFrame is a container for Series, and Series is a container for scalars. We would like to be able to insert and remove objects from these containers in a dictionary-like fashion.

## 2. Pandas Official Tutorials:
[>> 10 Minutes to pandas](http://pandas.pydata.org/pandas-docs/stable/10min.html#minutes-to-pandas)

[>> Pandas Tutorials](http://pandas.pydata.org/pandas-docs/stable/tutorials.html)

[>> Pandas Cookbook](http://pandas.pydata.org/pandas-docs/stable/tutorials.html#pandas-cookbook)

[>> Pandas Cheet Sheet](http://pandas.pydata.org/Pandas_Cheat_Sheet.pdf)

## 3. A Quick Start of Pandas - (based on [10 Minutes to pandas](http://pandas.pydata.org/pandas-docs/stable/10min.html#minutes-to-pandas))

First, in python, let's import the packages:
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

### 3.1 Object Creation
#### 3.1.1 Create a Series:
```python
In [3]: s = pd.Series([1,3,5,np.nan, 6, 8])
In [4]: s
Out[4]: 
0    1.0
1    3.0
2    5.0
3    NaN
4    6.0
5    8.0
dtype: float64
```
#### 3.1.2. Create a DataFrame
```python
In [6]: dates = pd.date_range('20130101', periods=6)

In [7]: dates
Out[7]:
DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04',
               '2013-01-05', '2013-01-06'],
              dtype='datetime64[ns]', freq='D')

In [8]: df = pd.DataFrame(np.random.randn(6,4), index=dates, columns=list('ABCD'))

In [9]: df
Out[9]:
                   A         B         C         D
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
2013-01-05 -0.424972  0.567020  0.276232 -1.087401
2013-01-06 -0.673690  0.113648 -1.478427  0.524988
```

<br><br>***KF***
