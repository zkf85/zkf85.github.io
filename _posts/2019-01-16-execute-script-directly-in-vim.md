---
layout: post
comments: true
title: "How to Execute a script directly within vi/vim"
date: "2019-01-16 12:53:00"
category: [Notes]
tag: [python, vim, how-to]
---

> Short Answer:

[>> Answer from Stack Overflow](https://stackoverflow.com/questions/3166413/execute-a-script-directly-within-vim-mvim-gvim)

You can do this in vim using the `!` command. For instance to count the number of words in the current file you can do:
```
:! wc %
```

The `%` is replaced by the current filename. To run a script you could call the interpreter on the file - for instance if you are writing a python script:
```
:! python %
```

<!--more-->


<br><br>***KF***
