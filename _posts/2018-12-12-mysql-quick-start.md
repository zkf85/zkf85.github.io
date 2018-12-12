---
layout: post
comments: true
title: "Quick Start on MySQL"
date: "2018-12-12 14:04:00"
category: [Database]
tag: [mysql, bash, linux]
---

> A quick start and a **Cheet Sheet** on MySQL.

<!--more-->

## 1. Install MySQL
[>> A Quick Guide to Using the MySQL APT Repository](https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/)

### 1.1. Install MySQL on Ubuntu 18.04 
#### 1.1.1. Install MySQL with APT:

- Just `apt install` it:
```bash
$ sudo apt install mysql-server
```

#### 1.1.2. Install MySQL with `.deb` installation package:
    
- Go to the download page for the MySQL APT repository at [https://dev.mysql.com/downloads/repo/apt/](https://dev.mysql.com/downloads/repo/apt/.).

- Install the downloaded package:
```bash
$ sudo dpkg -i /PATH/version-specific-package-name.deb
```

- Update package information:
```bash
$ sudo apt update
```

#### 1.1.3. Starting and Stopping MySQL Server
The MySQL server is started automatically after installation. 
- You can check the status of the MySQL server with the following command:
```bash
$ sudo service mysql status
```

- Stop the MySQL server with the following command:
```bash
$ sudo service mysql stop
```

- To restart the MySQL server, use the following command:
```bash
$ sudo service mysql start
```

### 1.2. Install MySQL on macOS
Just download the [MySQL disk image (`.dmg`) file](https://dev.mysql.com/downloads/mysql/) and install.

## 2. Most Frequently Used MySQL Commands

- See a help list:
```bash
$ mysql --help
```

### 2.1. Connecting/Disconnecting from the Server
- Connect to the server:
```bash
$ mysql -h host -u user -p
Enter password: ********
```

- If you are logging in on the same machine that MySQL is running on, you can simply:
```bash
$ mysql -u user -p
```

- After you have connected successfully, you can disconnect any time by typing `QUIT` (or `\q`) at the `mysql>` prompt:
```sql
mysql> QUIT
Bye
```
- On Unix, you can also disconnect by pressing Control+D.

### 2.2. Entering Queries
- Simple queries by entering:
```sql
mysql> SELECT VERSION(), CURRENT_DATE;
```

- Use queries as a calculator:
```mysql
mysql> SELECT SIN(PI()/4), (4+1)*5;
```

- If you decide you do not want to execute a query that you are in the process of entering, cancel it by typing `\c`:
```sql
mysql> SELECT
    -> USER()
    -> \c
mysql>
```

## References

[>> A Quick Guide to Using the MySQL APT Repository](https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/)
[>> MySQL 8.0 Reference Manual / Turorial](https://dev.mysql.com/doc/refman/8.0/en/tutorial.html)

<br><br>***KF***
