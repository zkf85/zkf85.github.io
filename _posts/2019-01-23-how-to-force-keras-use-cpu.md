---
layout: post
comments: true
title: "How to Force Keras to use CPU to Run Script?"
date: "2019-01-23 14:00:00"
category: [Deep Learning]
tag: [how-to, keras, tensorflow, deep learning, gpu, nvidia, cuda]
---

> **The reason for such a demand:**
> 
> My main training program was using the GPU fully. But I needed to get a prediction with another previously trained model urgently. I tried to use the GPU but I got OOM. Therefore, using CPU for the predicting job should be a good solution, and it did solve the problem!
> 
> Generally there are two ways: a short/lazy one and a lengthy but graceful one.


## Option I:
If you want to force Keras to use CPU
```python
import os
os.environ["CUDA_DEVICE_ORDER"] = "PCI_BUS_ID" 
os.environ["CUDA_VISIBLE_DEVICES"] = ""
```
before Keras / Tensorflow is imported.

<!--more-->

## Option II:
A rather graceful and separable way of doing this is to use
```python
import tensorflow as tf
from keras import backend as K

num_cores = 4

if GPU:
    num_GPU = 1
    num_CPU = 1
if CPU:
    num_CPU = 1
    num_GPU = 0

config = tf.ConfigProto(intra_op_parallelism_threads=num_cores,\
        inter_op_parallelism_threads=num_cores, allow_soft_placement=True,\
        device_count = {'CPU' : num_CPU, 'GPU' : num_GPU})
session = tf.Session(config=config)
K.set_session(session)
```
Here with `booleans` `GPU` and `CPU` you can specify whether to use a GPU or GPU when running your code.

The only thing to note is that you'll need `tensorflow-gpu` and `cuda/cudnn` installed because you're always giving the option of using a GPU.

## Reference
[>> Can Keras with Tensorflow backend be forced to use CPU or GPU at will? - Stack Overflow](https://stackoverflow.com/questions/40690598/can-keras-with-tensorflow-backend-be-forced-to-use-cpu-or-gpu-at-will)


<!--more-->

<br><br>***KF***
