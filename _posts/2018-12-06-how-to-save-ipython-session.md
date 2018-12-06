---
layout: post
comments: true
title: "How to Save IPython Session"
date: "2018-12-06 13:01:00"
category: [Python]
tag: [ipython, python, how-to]
---

> The detail of the question is described in this page [How to save a Python interactive session?]( https://stackoverflow.com/questions/947810/how-to-save-a-python-interactive-session)

<!--more-->

There are several ways to do it.

I'll list them below in the order of my preference.

### 1. `%logstart`
I prefer this method because it's automatic. You don't need to remember the saving stuff before you quit ipython. Simply type this command right after you open an `Ipython` session:
```python
%logstart
```
This will create a uniquely named .py file and store your session.

### 2. `%history`
This is also a better option. You just type in the magic command above rigth **before** you quit `Ipython`. everything in the session will be saved to python file you named.
```python
%history -f /tmp/history.py
```

### 3. `%save my_usefull_session 10-20 23`
This is the top answer to the question on stackoverflow. I don't prefer this way becuse it's not that easy to use.
Here's the description of this way:
> For example for your use-case there is the `%save` magic command, you just input `%save my_useful_session 10-20 23` to save input lines 10 to 20 and 23 to `my_useful_session.py` (to help with this, every line is prefixed by its number).

### 4. `readline`
Just another way by using python tool `readline`:
```python
import readline
readline.write_history_file('/home/ahj/history')
```

<br><br>***KF*** 
