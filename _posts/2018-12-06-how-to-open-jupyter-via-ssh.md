---
layout: post
comments: true
title: "How to Open Jupyter Notebook via SSH"
date: "2018-12-06 09:56:00"
category: [Operating System]
tag: [ssh, jupyter-notebook, python, how-to]
---

> The Objective is clear as described in the title. The case is usually encountered when logged in a remote server for computation but still want to use the jupyter-notebook that you're familiar with. This article tells how.

<!--more-->

## 1. Make It Happen
### 1.1 On Your Remote Server
Open a Jupyter Notebook with no-browser option:
```bash
kf@remote-server $ jupyter notebook --no-browser --port=8887
```

### 1.2. On Your Local Computer
Start an **SSH Tunnel** and connect it to the Jupyter notebook you just started on the server.
```bash
kf@local-computer $ ssh -N -L localhost:8888:localhost:8887 kf@remote-server
```

**`"-N"`** specifies not to execute a remote command. This is useful when forwarding ports.


**`"-L"`** binds the `local_address:port1` to a `remote_address:port2`. To be specific, it specifies that the connections for the socket on the local host are to be forwarded to the remote host. The socket then listens to the specified bind address.

## 2. Make It Simpler
To make things easier and save your time and "memory", use the default `port 8888` for both of your remote jupyter notebook and local binded port.

First, start the local tunnel:
```bash
kf@local-computer $ ssh -N -L localhost:8888:localhost:8888 kf@remote-server
```
> Note: Remember to keep this "tunnel" terminal alive!

Then, just start your jupyter notebook without specifying port number:
```bash
$ jupyter notebook --no-browser
```
Finally, copy the link you get from the terminal directly to a browser (since the port numbers are consistant), this link from terminal should be something like this:
```
Copy/paste this URL into your browser when you connect for the first time,
to login with a token:
    http://localhost:8888/?token=3e44d05a9258791304d98706d1c079971b098f60a9770a00&token=3e44d05a9258791304d98706d1c079971b098f60a9770a00
```

The good thing here is that you don't need to copy paste the lengthy token again and your jupyter is just ready to use like this:

![](/public/img/20181206-jupyter.png)

And we're done!

## Reference:
- [>> RUNNING JUPYTER NOTEBOOKS ON A REMOTE SERVER VIA SSH](https://techtalktone.wordpress.com/2017/03/28/running-jupyter-notebooks-on-a-remote-server-via-ssh/)

<br><br>***KF*** 
