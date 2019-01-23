---
layout: post
comments: true
title: "I Don't Like Jupyter Notebook"
date: "2019-01-16 09:45:00"
category: [Python]
tag: [ipython, jupyter-notebook]
---

> I've struggled with Jupyter Notebook for a long time and I have to write it down this time - I don't like it! And I feel relieved when I found that I'm not alone. And here're the reasons ... 

<!--more-->

## 1. "WE DON'T LIKE IT!"
Not only me feels that it's not a quite good tool in some way:

[>> Why I don't like Jupyter Notebooks - Dr Owain Kenway](https://owainkenwayucl.github.io/2017/10/03/WhyIDontLikeNotebooks.html)

[>> Why I Don't Like Jupyter (FKA IPython Notebook) - KIRILL POMOGAJKO](http://opiateforthemass.es/articles/why-i-dont-like-jupyter-fka-ipython-notebook/)

As for me, the reasons why I don't like to use this tool as a major tool for data analysis are:

## 2. And We Have Reasons:
I don't want to put a lot of word here like those two guys above. Here I just list the key points that Jupyter Notebooks annoyed me.

### 2.1. Code in Chunk is annoying!!
1. Code can only be run in chunks but not line by line. 
2. Order matters when running code, which could often bring problems.

### 2.2. File format incompatible with version control
It's json, not python source code. It's bad for version control.

### 2.3. In order to read your code/document people have to install Jupyter Notebook
Whenever you want to open and take a look at your code in such a notebook, you have to open a local web server and then open a browser. I'm a person preferring simplicity and I really hate this kind of experience.

## 3. My Solution:
- Keep using Vim only for coding in most cases.
- For exploring, use the REPL (Read-Eval-Print-Loop) tool iPython.
- Saving the plots in files as a default option in python codes.

<br><br>***KF***
