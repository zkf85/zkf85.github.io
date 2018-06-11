---
layout: post
title:  "@staticmethod和@classmethod的作用与区别"
category: [Python]
tag: [python]
---

<div class = "message">
@staticmethod和@classmethod的作用与区别
<br>KF 06/11/2018
</div>

### 相同点：
一般来说，要使用某个类的方法，需要先实例化一个对象再调用方法。
e.g.:

``` python
class A(object):
	bar = 1
	def foo(self):  
        print 'foo' 
	@staticmethod
	def static_foo():
		print("static_foo")
		
	@classmethod
	def class_foo():
		print("class_foo")
```

而使用`@staticmethod`或`@classmethod`，就可以不需要实例化，直接**类名.方法名()**来调用。e.g. `A.static_foo` 或者 `A.class_foo`

这有利于组织代码，把某些应该属于某个类的函数给放到那个类里去，同时有利于命名空间的整洁。

### 不同点：

既然@staticmethod和@classmethod都可以直接类名.方法名()来调用，那他们有什么区别呢

从它们的使用上来看,

- @staticmethod**不需要**表示自身对象的self和自身类的cls**参数**，就跟使用函数一样。

- @classmethod也不需要self参数，但**第一个参数**需要是表示自身类的cls参数。

如果在@staticmethod中要调用到这个类的一些属性方法，只能直接类名.属性名或类名.方法名, e.g. `A.bar`

而@classmethod因为持有cls参数，可以来调用类的属性，类的方法，实例化对象等，避免硬编码, e.g. `cls.bar`以及`cls().foo()`


#### 测试代码

```python
class A(object):  
    bar = 1  
    def foo(self):  
        print("oo")
 
    @staticmethod  
    def static_foo():  
        print("static_foo")
        print(A.bar)
 
    @classmethod  
    def class_foo(cls):  
        print("class_foo")
        print(cls.bar)
        cls().foo()  
  
A.static_foo()  
A.class_foo()  
```
输出

	static_foo
	1
	class_foo
	1
	foo

<br>***KF*** 
