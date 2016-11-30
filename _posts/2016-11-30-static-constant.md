---
layout: post
title: "What is the difference between a static and const variable"
date: "2016-11-30 14:42:00"
category: [iOS Development]
tags: [ios, objective-c]
---
<div class = "message">
Static VS. Constant
<br>KF 11/30/2016
</div>

A `static` variable means that the object's lifetime is the entire execution of the program and it's value is initialized only once before the program startup. All statics are initialized if you do not explicitly set a value to them.The manner and timing of static initialization is unspecified.

A `const` is a promise that you will not try to modify the value once set.

`const` is equivalent to `#define` but only for value statements(e.g. `#define myvalue 2`). The value declared replaces the name of the variable before compilation.

### Reference: 
[Static (C++) - on MSDN](https://msdn.microsoft.com/en-us/library/y5f6w579.aspx#static)

[static vs. const - on StackOverflow](http://stackoverflow.com/questions/2216239/what-is-the-difference-between-a-static-and-const-variable)

<br><br>
***KF***