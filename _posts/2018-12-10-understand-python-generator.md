---
layout: post
comments: true
title: "Understanding generators in Python"
date: "2018-12-10 12:52:00"
category: [Python, Deep Learning]
tag: [python, deep learning, keras]
---

> This post note a full understanding of a **[generator](http://www.python.org/dev/peps/pep-0255/)** in Python. 

<!--more-->

## 1. What is a Generator?
A **[generator](http://www.python.org/dev/peps/pep-0255/)** is a simply a function which returns an object on which you caqn call `next`, such that for every call it returns some value, until it raises a ` StopIteration` exception, signaling that all values have been generated. Such an object is called an *iterator*.

Normal functions return a single value using `return`. In Python, however, there's an alternative called `yield`. Using `yield` anywhere in a function makes it a generator:
```python
>>> def myGen(n):
...     yield n
...     yield n + 1
...
>>> g = myGen(6)
>>> next(g)
6
>>> next(g)
7
>>> next(g)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```
As you can see, `myGen(n)` is a function which yields `n` and `n+1`. Every call to [`next`](http://docs.python.org/3/library/functions.html#next) yields a single value, until all values have been yielded. `for` loops call `next` in the background, this:
```python
>>> for n in myGen(6):
...     print(n)
...
6
7
```

Likewise there are [generator expressions](http://www.python.org/dev/peps/pep-0289/), which provide a means to succinctly describe certain common types of generators:
```python
>>> g = (n for n in range(3, 5))
>>> next(g)
3
>>> next(g)
4
>>> next(g)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

Note that generator expressions are much like list [comprehensions](http://docs.python.org/3/tutorial/datastructures.html#list-comprehensions):
```python
>>> lc = [n for n in range(3, 5)]
>>> lc
[3, 4]
```

> **IMPORTANT**: A generator object is generated ***once***, but its code is not run all at once. Only calls to `next` actually execute (part of) the code. Execution of the code in a generator stops once a `yield` statement has been reached, upon which it returns a value. The next call to `next` then causes execution to continue in the state in which the generator was left after the last `yield`. This is a fundamental difference with regular functions: those always start execution at the "top" and discard their state upon returning a value.

## 2. Why use Generators?
There are a couple of good reasons:

- Certain concepts can be described much more succinctly using generators.

- Instead of creating a function which returns a list of values, one can write a generator which generates the values on the fly. This means that no list needs to be constructed, meaning that the resulting code is more **memory efficient**. In this way one can even describe data streams which would simply be too large to fit in memory.

- Generators allow for a natural way to describe infinite streams. Consider for example the [Fibonacci numbers](http://en.wikipedia.org/wiki/Fibonacci_number):
    ```python
    >>> def fib():
    ...     a, b = 0, 1
    ...     while True:
    ...         yield a
    ...         a, b = b, a + b
    ...
    >>> import itertools
    >>> list(itertools.islice(fib(), 10))
    [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
    ```

    This code uses `itertools.islice` to take a finite number of elements from an infinite stream. You are advised to have a good look at the functions in the `itertools` module, as they are essential tools for writing advanced generators with great ease.

## 3. Examples
Here is a good example of creating training/validation generators to feed the Keras `fit_generator` training process:
```python
# Create Training Generator
def trainGenerator(batch_size):
    # Generate data with batch_size
    while True:
        for i in range(0, train_size, batch_size):
            start_pos = i
            end_pos = min(i + batch_size, train_size)
            train_s1_X_batch = np.asarray(s1_training[start_pos:end_pos])
            train_s2_X_batch = np.asarray(s2_training[start_pos:end_pos])
            train_y_batch = np.asarray(label_training[start_pos:end_pos])
            # concatenate s1 and s2 data along the last axis
            train_concat_X_batch = np.concatenate([train_s1_X_batch, train_s2_X_batch], axis=-1)             
            # According to "fit_generator" on Keras.io, the output from the generator must
            # be a tuple (inputs, targets), thus,
            yield (train_concat_X_batch, train_y_batch)

# Create Valication Generator
def valGenerator(batch_size):
    while True:
        # Generate data with batch_size
        for i in range(0, val_size, batch_size):
            start_pos = i
            end_pos = min(i + batch_size, val_size)
            val_s1_X_batch = np.asarray(s1_validation[start_pos:end_pos])
            val_s2_X_batch = np.asarray(s2_validation[start_pos:end_pos])
            val_y_batch = np.asarray(label_validation[start_pos:end_pos])
            # concatenate s1 and s2 data along the last axis
            val_concat_X_batch = np.concatenate([val_s1_X_batch, val_s2_X_batch], axis=-1)
            # According to "fit_generator" on Keras.io, the output from the generator must
            # be a tuple (inputs, targets), thus,
            yield (val_concat_X_batch, val_y_batch)

train_size = 30000
val_size = 10000
batch_size = 32
epochs = 5

# Training loop with generators
model.fit_generator(
        trainGenerator(batch_size),
        steps_per_epoch=np.ceil(train_size/batch_size),
        epochs=epochs,
        validation_data=valGenerator(batch_size),
        validation_steps=np.ceil(val_size/batch_size)
        )
```

## References

[>> Understanding generators in Python on Stack Overflow](https://stackoverflow.com/questions/1756096/understanding-generators-in-python)


<br><br>***KF***
