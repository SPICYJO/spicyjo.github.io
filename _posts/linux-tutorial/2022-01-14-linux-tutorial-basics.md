---
layout: post
title: Linux Tutorial (1) - Basics
description: Basic commands you have to know when working with linux shell
author: Seungwoo Jo
last_modified_at: 2022-01-14 22:10:00 +0900
# math: false
tags: linux tutorial
category: Linux Tutorial
comments: true
---

## About this tutorial

Hello, guys! I am going to upload a series of tutorials of linux commands that can help you to grab an idea of how to use linux through a terminal(a.k.a. CLI). If you work in software field, I think you are quite familiar with these commands. In that case I would recommend you to skip familiar commands and jump to the commands you are interested in, because I am going to talk about lots of basic stuffs. If you are a beginner in software field, learning how to use a terminal is probably going to be very helpful for your future. And it also looks cool with all that fancy looking terminal screen, so why not try learning?

These are topics that I am going to talk about in this series.
- Linux commands (e.g. `ls`, `cd`, `mv`, etc.)
- Linux shells and shellscript
- Linux basic knowledges with a shallow look (e.g. `process`, `pid`, `user`, `group` ...)

And the followings are not going to be covered in this series.
- Mechanisms how linux works
- Deep dive into linux kernels
- How to make a flying car

Before you continue on, make sure that you have linux environment to try commands in the series.
- If you are using Mac, you can simply use the existing terminal.
- If you are using Linux, you can also just use the existing terminal.
- If you are using Windows, you can install WSL or Linux on your machine. Or you can connect to another linux environment via SSH.

## Overview
In this tutorial, I am going to talk about following basic commands.
- How to use shell?
- `ls`
- `cd`
- `pwd`
- `man`
- `cat`
- `mv`
- `cp`
- `rm`
- `touch`
- `mkdir`
- `rmdir`
- `which`
- `clear`

## Basic linux commands
#### How to use shell?
If it is your first time opening up a terminal, you probably feel kind of lost. But there is nothing to worry about. Using a terminal is very simple.

```bash
[spicyjo@spicyhost-spicydomain ~]$
```

Your terminal would look like above. It indicates your username, hostname and current directory. And the `$` sign means that you are going to make tons of money. I was just kidding. `$` sign means that you are logged in as a normal user. If you are logged in as a superuser, `#` sign will be displayed. 

You can type whatever command you want your computer to execute. Terminal is waiting for your command. You have to type right command so that your computer understands you. If you type a non-existing command, it will say something like below.

```bash
[spicyjo@spicyhost-spicydomain ~]$ showmethemoney
bash: showmethemoney: command not found...
[spicyjo@spicyhost-spicydomain ~]$
```

Your shell will try to execute the command you typed then it will return back to the standby state. Let's type pwd(with Enter).

```bash
[spicyjo@spicyhost-spicydomain ~]$ pwd
/home/spicyjo
[spicyjo@spicyhost-spicydomain ~]$
```

Yeah! We executed our first command. `pwd` shows the current working directory. So this is how you use your terminal. Let's try more commands.

#### ls
```bash
[spicyjo@spicyhost-spicydomain ~]$ ls
todo.txt
[spicyjo@spicyhost-spicydomain ~]$
```

`ls` list files and directory of the current working directory. You can also try it with options.

```bash
[spicyjo@spicyhost-spicydomain ~]$ ls -a
.  ..  .bash_logout  .bash_profile  .bashrc  .config  .esd_auth  .mozilla  .viminfo  todo.txt
[spicyjo@spicyhost-spicydomain ~]$ ls -l
total 4
-rw-rw-r--. 1 spicyjo spicyjo 5 Jan 11 10:12 todo.txt
[spicyjo@spicyhost-spicydomain ~]$
```

You can also type `ls -al` to combine options. There are lots of options, but I think `-a`, `-l` would be enough for now. If you want to figure out more available options, you can use `man` command which is going to be discussed below.

- `-a`: list all including hidden files or directories (files or directories starting with `.` is considered as hidden)
- `-l`: display each entry in a single line

#### cd
```bash
[spicyjo@spicyhost-spicydomain ~]$ ls -a
.  ..  .bash_logout  .bash_profile  .bashrc  .config  .esd_auth  .mozilla  .viminfo  todo.txt
[spicyjo@spicyhost-spicydomain ~]$ cd ..
[spicyjo@spicyhost-spicydomain home]$ ls
spicyjo  xyzman
[spicyjo@spicyhost-spicydomain home]$ ls -a
.  ..  spicyjo  xyzman
[spicyjo@spicyhost-spicydomain home]$ cd spicyjo/
[spicyjo@spicyhost-spicydomain ~]$ ls
```

`cd` changes your directory. `.` means current directory, and `..` means parent directory. `~` means home directory for the current user, `/home/spicyjo` in my case. `/` is the root directory. And another useful thing to know is that `-` means the previous directory.

#### pwd
```bash
[spicyjo@spicyhost-spicydomain ~]$ pwd
/home/spicyjo
[spicyjo@spicyhost-spicydomain ~]$
```

`pwd` prints current working directory.

#### man
```bash
[spicyjo@spicyhost-spicydomain ~]$ man pwd
PWD(1)                                   User Commands                                  PWD(1)

NAME
       pwd - print name of current/working directory

SYNOPSIS
       pwd [OPTION]...

DESCRIPTION
       Print the full filename of the current working directory.

       -L, --logical
              use PWD from environment, even if it contains symlinks

(...omitted)
```

`man` shows the manual for command. If you want to see a manual of `pwd` command, you can type `man pwd`. If it is your first time using this command, it could be hard to get out of that man page. You can press `q` to quit and `h` for help. `man` command is really helpful, so keep this in your mind. As an alternative, you can also use `--help` options for most of commands.

```bash
[spicyjo@spicyhost-spicydomain ~]$ pwd --help
pwd: pwd [-LP]
    Print the name of the current working directory.

    Options:
      -L        print the value of $PWD if it names the current working
                directory
      -P        print the physical directory, without any symbolic links

    By default, `pwd' behaves as if `-L' were specified.
```

#### cat
```bash
[spicyjo@spicyhost-spicydomain ~]$ cat todo.txt
- Wash dishes
- Go to gym
- Write a blog post
[spicyjo@spicyhost-spicydomain ~]$ cat todo.txt todo.txt
- Wash dishes
- Go to gym
- Write a blog post
- Wash dishes
- Go to gym
- Write a blog post
[spicyjo@spicyhost-spicydomain ~]$
```

`cat` command concatenates files and prints on the standard output.

#### mv
```bash
[spicyjo@spicyhost-spicydomain ~]$ ls
todo.txt
[spicyjo@spicyhost-spicydomain ~]$ mv todo.txt todo2.txt
[spicyjo@spicyhost-spicydomain ~]$ ls
todo2.txt
[spicyjo@spicyhost-spicydomain ~]$
```

`mv` command moves file or directory. (directory is regarded as file in linux system) You can change the file's name or path.

#### cp
```bash
[spicyjo@spicyhost-spicydomain ~]$ ls
todo2.txt
[spicyjo@spicyhost-spicydomain ~]$ cp todo2.txt newtodo.txt
[spicyjo@spicyhost-spicydomain ~]$ ls
newtodo.txt  todo2.txt
[spicyjo@spicyhost-spicydomain ~]$
```

`cp` command copies file.

#### rm
```bash
[spicyjo@spicyhost-spicydomain ~]$ ls
newtodo.txt  todo2.txt
[spicyjo@spicyhost-spicydomain ~]$ rm todo2.txt
[spicyjo@spicyhost-spicydomain ~]$ ls
newtodo.txt
[spicyjo@spicyhost-spicydomain ~]$
```

`rm` command removes file.

#### touch
```bash
[spicyjo@spicyhost-spicydomain ~]$ ls
newtodo.txt
[spicyjo@spicyhost-spicydomain ~]$ touch test.txt
[spicyjo@spicyhost-spicydomain ~]$ ls
newtodo.txt  test.txt
[spicyjo@spicyhost-spicydomain ~]$ ls -l
total 4
-rw-rw-r--. 1 spicyjo spicyjo 46 Jan 13 10:29 newtodo.txt
-rw-rw-r--. 1 spicyjo spicyjo  0 Jan 13 10:31 test.txt
[spicyjo@spicyhost-spicydomain ~]$ touch test.txt
[spicyjo@spicyhost-spicydomain ~]$ ls -l
total 4
-rw-rw-r--. 1 spicyjo spicyjo 46 Jan 13 10:29 newtodo.txt
-rw-rw-r--. 1 spicyjo spicyjo  0 Jan 13 10:32 test.txt
[spicyjo@spicyhost-spicydomain ~]$
```

`touch` command changes file timestamps and create file if not exist.

#### mkdir
```bash
[spicyjo@spicyhost-spicydomain ~]$ ls
newtodo.txt  test.txt
[spicyjo@spicyhost-spicydomain ~]$ mkdir tutorial
[spicyjo@spicyhost-spicydomain ~]$ ls
newtodo.txt  test.txt  tutorial
[spicyjo@spicyhost-spicydomain ~]$
```

`mkdir` command makes directory.

#### rmdir
```bash
[spicyjo@spicyhost-spicydomain ~]$ ls
newtodo.txt  test.txt  tutorial
[spicyjo@spicyhost-spicydomain ~]$ rmdir tutorial
[spicyjo@spicyhost-spicydomain ~]$ ls
newtodo.txt  test.txt
[spicyjo@spicyhost-spicydomain ~]$
```

`rmdir` command removes directory.

#### which
```bash
[spicyjo@spicyhost-spicydomain ~]$ which rmdir
/usr/bin/rmdir
[spicyjo@spicyhost-spicydomain ~]$
```

`which` command shows where the command is located at. Each linux command is a program, so basically it is an executable file.

#### clear
```bash
[spicyjo@spicyhost-spicydomain ~]$ clear
```

`clear` command clears screen output. It is handy when you want to clean up messy output.

## Wrap up
In this article, we have learned the following basic commands. Try them on your terminal by yourself so that you can get used to them. I'll see you in the next one. 

- `ls`
- `cd`
- `pwd`
- `man`
- `cat`
- `mv`
- `cp`
- `rm`
- `touch`
- `rmdir`
- `mkdir`
- `which`
- `clear`