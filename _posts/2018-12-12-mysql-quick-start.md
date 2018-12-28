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
> **Note**: "`QUIT`" doesn't need "`;`".

- On Unix, you can also disconnect by pressing Control+D.

### 2.2. Entering Queries
- Simple queries by entering:
```sql
mysql> SELECT VERSION(), CURRENT_DATE;
```

- Use queries as a calculator:
```sql
mysql> SELECT SIN(PI()/4), (4+1)*5;
```

- If you decide you do not want to execute a query that you are in the process of entering, cancel it by typing `\c`:
```sql
mysql> SELECT
    -> USER()
    -> \c
mysql>
```

### 2.3. Creating and Using a Database
- Find out what databases currently exist on the server:
```sql
mysql> SHOW DATABASES;
```
    > The output:
    ```
+----------+
| Database |
+----------+
| mysql    |
| test     |
| tmp      |
+----------+
```

- Access a listed database:
```sql
mysql> USE test
```
> **Note**: "`USE`" doesn't need "`;`".

- To change the permission of a database to a single user:
```sql
mysql> GRANT ALL ON menagerie.* TO 'your_mysql_name'@'your_client_host';
```
#### 2.3.1 Create and Select a Database
- Create a Database:
```sql
mysql> CREATE DATABASE mydatabase;
```
> **Note**: Your database needs to be created only once, but you must select it for use each time you begin a mysql session. (Use the statement: `USE mydatabase`)
> 
> Alternatively, specify its name after any connection parameters when logging in to MySQL:
```bash
$ mysql -h host -u user -p mydatabase
Enter password: ********
```

#### 2.3.2. Create a Table
- Show all tables in the current database:
```sql
mysql> SHOW TABLES;
```

- Create a table, use a `CREATE TABLE` statement to specify the layout of your table:
```sql
mysql> CREATE TABLE pet (name VARCHAR(20), owner VARCHAR(20),
    -> species VARCHAR(20), sex CHAR(1), birth DATE, death DATE);
```
> **Note**: You can check [All Data Types](https://dev.mysql.com/doc/refman/8.0/en/data-types.html) to pick an appropriate one for each item in the table.

- Once you have created a table, SHOW TABLES should produce some output:
```sql
mysql> SHOW TABLES;
+----------------------+
| Tables_in_mydatabase |
+----------------------+
| pet                  |
+----------------------+
```

- Show information about the table structure:
```sql
mysql> DESCRIBE pet;
```

    > The output would be like this:
    ```
+---------+-------------+------+-----+---------+-------+
| Field   | Type        | Null | Key | Default | Extra |
+---------+-------------+------+-----+---------+-------+
| name    | varchar(20) | YES  |     | NULL    |       |
| owner   | varchar(20) | YES  |     | NULL    |       |
| species | varchar(20) | YES  |     | NULL    |       |
| sex     | char(1)     | YES  |     | NULL    |       |
| birth   | date        | YES  |     | NULL    |       |
| death   | date        | YES  |     | NULL    |       |
+---------+-------------+------+-----+---------+-------+
```

#### 2.3.3. Loading Data into a Table
- To load the text file `pet.txt` into the pet table, use this statement:
```sql
mysql> LOAD DATA LOCAL INFILE '/path/pet.txt' INTO TABLE pet;
```

- When you want to add new records one at a time, use `INSERT` statement like this:
```sql
mysql> INSERT INTO pet
    -> VALUES ('Puffball','Diane','hamster','f','1999-03-30',NULL);
```

#### 2.3.4. Retrieving Information from a Table
- The `SELECT` statement is used to pull information from a table. The general form of the statement is:
```sql
SELECT what_to_select
FROM which_table
WHERE conditions_to_satisfy;
```

- Select All data from a table:
```sql
mysql> SELECT * FROM pet;
```

- Empty the table:
```sql
mysql> DELETE FROM pet;
```

- Change a certain record with `UPDATE` statement:
```sql
mysql> UPDATE pet SET birth = '1989-08-31' WHERE name = 'Bowser';
```

- [>> Selecting Particular Rows](https://dev.mysql.com/doc/refman/8.0/en/selecting-rows.html)
- [>> Selecting Particular Columns](https://dev.mysql.com/doc/refman/8.0/en/selecting-columns.html)
- [>> Sorting Rows](https://dev.mysql.com/doc/refman/8.0/en/sorting-rows.html)
- [>> Date Calculations](https://dev.mysql.com/doc/refman/8.0/en/date-calculations.html)
- [>> Working with NULL Values](https://dev.mysql.com/doc/refman/8.0/en/working-with-null.html)
- [>> Pattern Matching](https://dev.mysql.com/doc/refman/8.0/en/pattern-matching.html)
- [>> Couting Rows](https://dev.mysql.com/doc/refman/8.0/en/counting-rows.html)
- [>> Using More Than one Table](https://dev.mysql.com/doc/refman/8.0/en/multiple-tables.html)

### 2.4. Using mysql in Batch Mode
- To run a mysql script from file:
```bash
$ mysql < batch-file
```

- If connection parameters needed, the command might look like this:
```bash
$ mysql -h host -u user -p < batch-file
Enter password: ********
```

- Run the output through a pager:
```bash
$ mysql < batch-file | less
```

- Catch the output in a log file:
```bash
$ mysql < batch-file > mysql.out
```

- You can also use scripts from the mysql prompt by using the source command or \. command:
```sql
mysql> source filename;
mysql> \. filename
```

## 3. MySQL Security Issues
- [>> MySQL Tutorial on Security Issues](https://dev.mysql.com/doc/refman/8.0/en/security.html)

### 3.1. Security Guidelines
- ***Do not ever give anyone (except MySQL `root` accounts) access to the `user` table in the mysql system database! This is critical***.

- ***Set a root password!*** Try **`mysql -u root`**, if you are able to connect successfully, then you should set a password for `root`.

- Use the **[`SHOW GRANTS`](https://dev.mysql.com/doc/refman/8.0/en/show-grants.html)** statement to check which accounts have access to what. Then use the **[`REVOKE`](https://dev.mysql.com/doc/refman/8.0/en/revoke.html)** statement to remove thos privileges that are not necessary.



## References

[>> A Quick Guide to Using the MySQL APT Repository](https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/)
[>> MySQL 8.0 Reference Manual / Turorial](https://dev.mysql.com/doc/refman/8.0/en/tutorial.html)


<br><br>***KF***
