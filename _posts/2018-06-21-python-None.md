---
layout: post
title: "Python None"
date: "2018-06-21 14:00:00"
category: [Python]
tag: [python]
---

[baicaiye专栏](https://blog.csdn.net/baicaiye/article/details/72922197)

None是一个特殊的常量。
None和False不同。
None不是0。
None不是空字符串。
None和任何其他的数据类型比较永远返回False。
None有自己的数据类型NoneType。
你可以将None复制给任何变量，但是你不能创建其他NoneType对象。




[Python NoneType - python.org](https://docs.python.org/3/c-api/none.html)

##The None Object

Note that the PyTypeObject for None is not directly exposed in the Python/C API. Since None is a **singleton**, testing for object identity (using == in C) is sufficient. There is no PyNone_Check() function for the same reason.

### `PyObject* Py_None`

The Python None object, denoting lack of value. This object has no methods. It needs to be treated just like any other object with respect to reference counts.

### `Py_RETURN_NONE`

Properly handle returning Py_None from within a C function (that is, increment the reference count of None and return it.)



<br>***KF*** 
