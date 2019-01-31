---
layout: post
comments: true
title: "Random! Shuffle v.s. Permutation (Numpy)"
date: "2019-01-31 10:40:00"
category: [Python]
tag: [numpy, random]
---
> Generally, in Numpy, both `random.permutation` and `random.shuffle` randomly shuffle elements in an array. But there are differences: 

## Difference:
`np.random.permutation` has two differences from `np.random.shuffle`:

if passed an array, it will return a shuffled **copy** of the array; `np.random.shuffle` shuffles the array **inplace**

if passed an integer, it will return a shuffled range i.e. `np.random.shuffle(np.arange(n))`

If x is an integer, randomly permute `np.arange(x)`. If x is an array, make a copy and shuffle the elements randomly.

The source code might help to understand this:
```python
3280        def permutation(self, object x):
...
3307            if isinstance(x, (int, np.integer)):
3308                arr = np.arange(x)
3309            else:
3310                arr = np.array(x)
3311            self.shuffle(arr)
3312            return arr
```

## Reference
[>> shuffle vs permute numpy - Stack Overflow](https://stackoverflow.com/questions/15474159/shuffle-vs-permute-numpy)

<!--more-->

<br><br>***KF***
