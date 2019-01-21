---
layout: post
comments: true
title: "How to Set Static IP Address on Ubuntu 18.04 Using Netplan"
date: "2018-12-10 09:18:00"
category: [Operating System]
tag: [how-to, ubuntu, linux, netplan]
---

> Recently I was struggling with tensorflow-gpu installation my home server. After couple of rounds of attempts and re-installation. I have to choose Ubuntu-18.04 only (due to my mother board version, both ubuntu-16.04 and debian-9 have network driver issues). 

> During the time when re-installing the system, setting a static IP address is a common routine that make other computer in the home network easy to access the server. Here I'll show how to configure static IP Address in Ubuntu 18.04 with netplan.

<!--more-->

- When installing Ubuntu-18.04, a DHCP is set by default.
[>> See what is DHCP (Dynamic Host Configuration Protocol](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol)

- After the installation of the system, we'll see how to configure static IP address in Ubuntu-18.04 with **[netplan - new network configuration tool](https://netplan.io)**.

- Besides, a tradidional method is also shown later in the article, by using **`ifupdown`** for the same job.


[>> Netplan – How To Configure Static IP Address in Ubuntu 18.04 using Netplan](https://www.itzgeek.com/how-tos/linux/ubuntu-how-tos/netplan-how-to-configure-static-ip-address-in-ubuntu-18-04-using-netplan.html)


## 1. Configure Static IP Address with Netplan in Ubuntu 18.04
### 1.1. Find your desired network interface
Use one of the following commands to find the network interfaces available on you system.
```sh
$ ifconfig -a
```
or
```sh
$ ip a
```

The output would be like this:
```
enp2s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.41 netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::201:6cff:fed0:e0e9  prefixlen 64  scopeid 0x20<link>
        ether 00:01:6c:d0:e0:e9  txqueuelen 1000  (Ethernet)
        RX packets 10356  bytes 8636081 (8.6 MB)
        RX errors 0  dropped 13  overruns 0  frame 0
        TX packets 3322  bytes 329660 (329.6 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 144  bytes 10868 (10.8 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 144  bytes 10868 (10.8 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

Now, the system interface **enp2s0** takes IP address from DHCP server.

In this article, I'll configure a static IP address for enp2s0 as:
```
IP address = 192.168.1.100
Netmask = 255.255.255.0
Gateway = 192.168.1.1
```

### 1.2. Disable `cloud-init`
In Ubuntu-18.04 server, `cloud-init` manages the network configuration. So you need to disable it by adding such a file:
```sh
$ sudo vi /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
```
Add the following line int the configuration file:
```yaml
network: {config: disabled}
```

### 1.3. Backup Default Netplan Files
Move any files present in `/etc/netplan` directory to a backup location:
```sh
$ sudo mv /etc/netplan/* /root
```

### 1.4. Create a New Netplan Configuration File
Create a new configuration file by:
```sh
$ sudo vim /etc/netplan/01-network-card.yaml
```

Add the following content to the file:
```yaml
network:
        version: 2
        renderer: networkd
        ethernets:
                enp0s3:
                        dhcp4: no
                        addresses: [192.168.1.100/24]
                        gateway4: 192.168.1.1
                        nameservers:
                                addresses: [192.168.1.1,8.8.8.8]

```

### 1.5. Apply All Configuration and Restart Renderers
```sh 
$ sudo netplan apply
```

### 1.6. Verify the Change
Now verify the static IP address by:
```sh
$ ifconfig -a
```
or
```sh
$ ip a
```
You'll see:
```
enp2s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.235  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::201:6cff:fed0:e0e9  prefixlen 64  scopeid 0x20<link>
        ether 00:01:6c:d0:e0:e9  txqueuelen 1000  (Ethernet)
        RX packets 11530  bytes 8736376 (8.7 MB)
        RX errors 0  dropped 14  overruns 0  frame 0
        TX packets 3488  bytes 362346 (362.3 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

## 2. Configure Static IP Address using `ifupdown`/`Network Manager`

### 2.1. Install Packages
Install the below packages to support the old method of configuring static IP address:
```sh 
$ sudo apt install ifupdown resolvconf
```

### 2.2. Ethernet
Edit the interfaces file:
```sh
$ sudo vim /etc/network/interfaces
```
Update the file with the following information
```sh
# Interface Name #
auto enp0s3
# Static IP Address #
iface enp0s3 inet static
# IP Address #
address 192.168.1.100
# Netmask #
netmask 255.255.255.0
# Gateway #
gateway 192.168.1.1
# DNS Servers #
#dns-nameservers 192.168.1.1
#dns-nameservers 8.8.8.8
# Search Domain #
#dns-search itzgeek.local
```
> **Note**: you can also configure your DNS Server just by uncommenting the last three dns related configuration lines.

### 2.3. Restart and Apply
Restart the networking using the following command:
```sh
$ sudo systemctl restart networking
```

You're all set now!

<br><br>***KF*** 
