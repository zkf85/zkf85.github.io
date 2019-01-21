---
layout: post
comments: true
title: "What does 'rc' Mean in Unix-like systems, or even in Matplotlib rcParams?"
date: "2019-01-17 12:30:00"
category: [Python, Operation System]
tag: [python, unix, linux, matplotlib]
---
> In the context of Unix-like systems, the term rc stands for the phrase **"run commands"**. It is used for any file that contains startup information for a command. It is believed to have originated somewhere in 1965 from a runcom facility from the MIT Compatible Time-Sharing System (CTSS).


From Brian Kernighan and Dennis Ritchie:

> There was a facility that would execute a bunch of commands stored in a file; it was called runcom for "run commands", and the file began to be called "a runcom". rc in Unix is a fossil from that usage.

<!--more-->

Tom Van Vleck, a Multics engineer, has also reminisced about the extension rc: "The idea of having the command processing shell be an ordinary slave program came from the Multics design, and a predecessor program on CTSS by Louis Pouzin called RUNCOM, the source of the '.rc' suffix on some Unix configuration files."

This is also the origin of the name of the Plan 9 from Bell Labs shell by Tom Duff, the rc shell. It is called "rc" because the main job of a shell is to "run commands".

While not historically precise, rc may also be expanded as "run control", because an rc file controls how a program runs. For instance, the editor Vim looks for and reads the contents of the .vimrc file to determine its initial configuration. In The Art of Unix Programming, Eric S. Raymond consistently refers to rc files as "run-control" files.

## Reference
[>> Run commands - Wikipedia](https://en.wikipedia.org/wiki/Run_commands)


<br><br>***KF***
