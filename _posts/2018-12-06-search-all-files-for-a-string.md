---
layout: post
comments: true
title: "Linux/Unix Recursively Search All Files for a String"
date: "2018-12-06 13:15:00"
category: [Operating System]
tag: [linux, unix, macOS, grep, find, shell, bash]
---

> A quick answer here: 

```bash
$ grep -r "string" .
```
<!--more-->
Check this link:
[>> Linux / UNIX Recursively Search All Files For A String](https://www.cyberciti.biz/faq/howto-recursively-search-all-files-for-words/)

## `grep` command: Recursively Search All Files For A String

The syntax is:
```bash
$ cd /path/to/dir
$ grep -r "word" .
```
```bash
$ To ignore case distinctions:
$ grep -ri "word" .
```

To display print only the filenames with GNU grep, enter:
```bash
$ grep -r -l "foo" .
```

You can also specify directory name:
```bash
$ grep -r -l "foo" /path/to/dir/*.c
```

## `find` command: Recursively Search All Files For A String

`find` command is recommend because of speed and ability to deal with filenames that contain spaces.

```bash
cd /path/to/dir
find . -type f -exec grep -l "word" {} +
find . -type f -exec grep -l "seting" {} +
find . -type f -exec grep -l "foo" {} +
find /search/dir/ -type f -name "*.c" -print0 | xargs -I {} -0 grep "foo" "{}"

## Search /etc/ directory for 'nameserver' word in all *.conf files ##
find /etc/ -type f -name "*.conf" -print0 | xargs -I {} -0 grep "nameserver" "{}"
```

Older UNIX version should use xargs to speed up things:
```bash
$ find /path/to/dir -type f | xargs grep -l "foo"
```

It is good idea to pass -print0 option to find command that it can deal with filenames that contain spaces or other metacharacters:
```bash
$ find /path/to/dir -type f -print0 | xargs -0 grep -l "foo"
```

OR use the following OSX/BSD/find or GNU/find example:

```bash
find /path/to/dir/ -type f -name "file-pattern" -print0 | xargs -I {}  -0 grep -l "search-term" "{}"

## OR ##
find /mycool/project/ -type f -name "*.py" -print0 | xargs -I {}  -0 grep -H --color "methodNameHere" "{}"

## OR search all files in /etc/ dir for 'nameserver' word ##
find /etc/ -iname "*" -type f -print0  |  xargs -0 grep -H "nameserver"
```

<br><br>***KF*** 
