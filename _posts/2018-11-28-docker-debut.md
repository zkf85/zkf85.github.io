---
layout: post
comments: true
title: "Get Started with THE Docker"
date: "2018-11-28"
category: [Operating System]
tag: [docker]
---

> The famous "Docker" is ***a tool that can package an application and tis dependencies in a virtual container that can run on any Linux server***. After getting my keras model, it's a good chance to try docker to deploy a deep learning model as a API service. In this article, I'd like to write down all the important procedures and details about how to make this service work with docker."
 
![](/public/img/20181128-laurel-docker-containers.png)

 <!--more-->

## 1. Check out Docker!
Of course we can install and configure the "*Flask/uWSGI/NGINX (FUN)*" combo step by step, but it's trivial and with bunch of pitfalls.

Docker is the perfect choice to make such things much easier!

### 1.1. Basic idea of using Docker
The basic idea of using Docker is to avoid installing and configuring environment by yourself. Instead, you can download a ready-to-use ***image***, which is carefully configured and saved by other people, and run it as a ***container***, in which you can focus on you own stuff without worrying about whether the environment trivials at all.


## 2. Install/Uninstall Docker
Docker is available in two editions:
- Community Edition (CE)
- Enterprise Edition (EE)

The CE edition is suitable for individual developers and it's free.

To get Docker CE for Ubuntu, check the instruction [here](https://docs.docker.com/install/linux/docker-ce/ubuntu/).

### 2.1. Prerequisites
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

### 2.2. Install Docker CE (using repository)
1. Update the `apt` package index:

	```sh
	$ sudo apt-get update
	```

2. Install packages to allow apt to use a repository over HTTPS:

	```sh
	$ sudo apt-get install \
		apt-transport-https \
		ca-certificates \
		curl \
		software-properties-common
	```

3. Install the *latest* version of Docker CE, or go to the next step to install a specific version:

	```sh
	$ sudo apt-get install docker-ce
	```

4. Verify that Docker CE is installed correctly by running the hello-world image.

	```sh
	$ sudo docker run hello-world
	```

### 2.3. Uninstall Docker CE
1. Uninstall the Docker CE package:

	```sh
	$ sudo apt-get purge docker-ce
	```

2. Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

	```sh
	$ sudo rm -rf /var/lib/docker
	```

### 2.3. Other installation methods
You can also install Docker CE from *a package* or *using the convenience script*. Again, for details check [here](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-repository)!

## 3. Docker basics
Docker official site provides a very nice set of [Tutorials](https://docs.docker.com/get-started/). It's highly recommended to go through the first couple of sections of the tutorial. Here I'd like to keep a record of the very basic steps and commands to get started with Docker.

The best and complete way to learn Docker is to go through this [Get Started from Docker's Official](https://docs.docker.com/get-started/). Here I just take a record of the core concepts and commands for myself.

### 3.1. Docker Concepts

- Image:

	An **image** is an executable package that includes everything needed to run an application--the code, a runtime, libraries, environment variables, and configuration files.	

- Container:

	A **container** is a runtime instance of an image--what the image becomes in memory when executed (that is, an image with state, or a user process). You can see a list of your running containers with the command, `docker ps`, just as you would in Linux.

- Container vs. Virtual Machines:

A **container** runs natively on Linux and shares the kernel of the host machine with other containers. It runs a discrete process, taking no more memory than any other executable, making it lightweight.

By contrast, a **virtual machine (VM)** runs a full-blown “guest” operating system with virtual access to host resources through a hypervisor. In general, VMs provide an environment with more resources than most applications need.

<table>
  <tr>
    <td>
    <img src="/public/img/20181128-docker-container.png">
    </td>

    <td>
    <img src="/public/img/20181128-docker-VM.png">
	</td>
  </tr>
</table>

### 3.2. **Cheat sheet 1 - Check info**

```sh
## List Docker CLI commands
docker
docker container --help

## Display Docker version and info
docker --version
docker version
docker info

## Execute Docker image
docker run hello-world

## List Docker images
docker image ls

## List Docker containers (running, all, all in quiet mode)
docker container ls
docker container ls --all
docker container ls -aq

```

### 3.3. **Cheat sheet 2 - Check info**

After `Dockerfile` and `app.py` (optionally `requirements`) are ready:

```sh

docker build -t friendlyhello .  # Create image using this directory's Dockerfile
docker run -p 4000:80 friendlyhello  # Run "friendlyname" mapping port 4000 to 80
docker run -d -p 4000:80 friendlyhello         # Same thing, but in detached mode
docker container ls                                # List all running containers
docker container ls -a             # List all containers, even those not running
docker container stop <hash>           # Gracefully stop the specified container
docker container kill <hash>         # Force shutdown of the specified container
docker container rm <hash>        # Remove specified container from this machine
docker container rm $(docker container ls -a -q)         # Remove all containers
docker image ls -a                             # List all images on this machine
docker image rm <image id>            # Remove specified image from this machine
docker image rm $(docker image ls -a -q)   # Remove all images from this machine
docker login             # Log in this CLI session using your Docker credentials
docker tag <image> username/repository:tag  # Tag <image> for upload to registry
docker push username/repository:tag            # Upload tagged image to registry
docker run username/repository:tag                   # Run image from a registry

```
### 3.3. **Cheat sheet 3 - Services**

```sh

docker stack ls                                            # List stacks or apps
docker stack deploy -c <composefile> <appname>  # Run the specified Compose file
docker service ls                 # List running services associated with an app
docker service ps <service>                  # List tasks associated with an app
docker inspect <task or container>                   # Inspect task or container
docker container ls -q                                      # List container IDs
docker stack rm <appname>                             # Tear down an application
docker swarm leave --force      # Take down a single node swarm from the manager

```

<br><br>***KF*** 
