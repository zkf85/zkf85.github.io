---
layout: post
comments: true
title: "Get Started with THE Docker"
date: "2018-11-28"
category: [Operating System]
tag: [docker]
---

>The famous "Docker" is ***a tool that can package an application and tis dependencies in a virtual container that can run on any Linux server***. After getting my keras model, it's a good chance to try docker to deploy a deep learning model as a API service. In this article, I'd like to write down all the important procedures and details about how to make this service work with docker."
 
 <!--more-->

## 1. Background
### 1.1. Description of my job
After spending more than 200 credits on google cloud GPU for training a **Plant Disease Recoginition (PDR)** model with Keras, I've got a decent model that reached more than 85% accuracy. The model is ready and the next thing to do is to deploy the model for inference as a service.

The main objective is to build a web service API which responds with a inference result as a json file after receiving a request with an image in it.

### 1.2. What we have?
At the moment, we have a well-built keras model, trained with `keras==2.2.4` and `tensorflow==1.11.0` in `python==3.5.2`.

### 1.3. What strategy we choose?
Basically, it's **"Flask + uWSGI + NGINX"**:

- [Flask](http://flask.pocoo.org) is a good python microframework for web development. It is pretty easy to make an improvised API with Flask. But it's not recommended to use it to build a formal production.
- [uWSGI](https://uwsgi-docs-additions.readthedocs.io/en/latest/) aims at developing a full stack for building hosting services. uWSGI is implemented as a linker between Nginx(does not support python) and Flask(written in python).
- [NGINX](https://www.nginx.com/resources/wiki/)  ( [/ˌɛndʒɪnˈɛks/ EN-jin-EKS](https://en.wikipedia.org/wiki/Nginx)) is a free, open-source, high-performance HTTP server and reverse proxy.

## 2. Docker time!
Of course we can install and configure the "*Flask/uWSGI/NGINX (FUN)*" combo step by step, but it's trivial and with bunch of pitfalls.

Docker is the perfect choice to make such things much easier!

### 2.1. Basic idea of using Docker
The basic idea of using Docker is to avoid installing and configuring environment by yourself. Instead, you can download a ready-to-use ***image***, which is carefully configured and saved by other people, and run it as a ***container***, in which you can focus on you own stuff without worrying about whether the environment trivials at all.


### 2.2. Install/Uninstall Docker
Docker is available in two editions:
- Community Edition (CE)
- Enterprise Edition (EE)

The CE edition is suitable for individual developers and it's free.

To get Docker CE for Ubuntu, check the instruction [here](https://docs.docker.com/install/linux/docker-ce/ubuntu/).

#### 2.2.1. Prerequisites
1. Uninstall old versions:
```sh
$ sudo apt-get remove docker docker-engine docker.io
```

2. Update the `apt` package index:
```sh
$ sudo apt-get update
```

3. Install packages to allow apt to use a repository over HTTPS:
```sh
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```

4. Add Docker’s official GPG key:

	```sh
	$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
	```

	Verify that you now have the key with the fingerprint `9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88`, by searching for the last 8 characters of the fingerprint.

	```sh
	$ sudo apt-key fingerprint 0EBFCD88

	pub   4096R/0EBFCD88 2017-02-22
		  Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
	uid                  Docker Release (CE deb) <docker@docker.com>
	sub   4096R/F273FCD8 2017-02-22
	```

5. Use the following command to set up the stable repository. ( For x86_64/amd64)
	```sh
	$ sudo add-apt-repository \
	   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
	   $(lsb_release -cs) \
	   stable"
	```

#### 2.2.2. Install Docker CE
1. Update the `apt` package index:

	```sh
	$ sudo apt-get update
	```

2. Install packages to allow apt to use a repository over HTTPS:

	```sh
	$ sudo apt-get update
	```

3. Install the *latest* version of Docker CE, or go to the next step to install a specific version:

	```sh
	$ sudo apt-get install docker-ce
	```

4. Verify that Docker CE is installed correctly by running the hello-world image.

	```sh
	$ sudo docker run hello-world
	```

### 2.2.3. Uninstall Docker CE
1. Uninstall the Docker CE package:

	```sh
	$ sudo apt-get purge docker-ce
	```

2. Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

	```sh
	$ sudo rm -rf /var/lib/docker
	```

### 2.3. Docker basics
Docker official site provides a very nice set of [Tutorials](https://docs.docker.com/get-started/). It's highly recommended to go through the first couple of sections of the tutorial. Here I'd like to keep a record of the very basic steps and commands to get started with Docker.






<br><br>***KF*** 
