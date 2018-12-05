---
layout: post
comments: true
title: "How to Open Jupyter Notebook via SSH"
date: "2018-12-05 17:09:00"
category: [Operating System]
tag: [ssh, jupyter-notebook]
---

> The Objective is clear as described in the title. The case is usually encountered when logged in a remote server for computation but still want to use the jupyter-notebook that you're familiar with. This article tells how.

<!--more-->

## 1. On Your Remote Server
Open a Jupyter Notebook with no-browser option:
```bash
kf@remote-server $ jupyter notebook --no-browser --port=8887
```

## 2. On Your Local Computer
Start a ~~SSH Tunnel~~ and connect it to the Jupyter notebook you just started on the server.
```bash
kf@local-computer $ ssh -N -L localhost:8888:localhost:8887 kf@remote-server
```

## Reference:
- [>> RUNNING JUPYTER NOTEBOOKS ON A REMOTE SERVER VIA SSH](https://techtalktone.wordpress.com/2017/03/28/running-jupyter-notebooks-on-a-remote-server-via-ssh/)

<br><br>***KF*** 
