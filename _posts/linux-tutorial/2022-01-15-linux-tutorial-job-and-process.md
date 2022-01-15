---
layout: post
title: Linux Tutorial (2) - Job and Process
description: Linux commands to deal with job and process
author: Seungwoo Jo
# last_modified_at: 2022-01-14 22:10:00 +0900
# math: false
tags: linux tutorial
category: Linux Tutorial
comments: true
---

## Overview
In this tutorial, I am going to talk about following commands which can help you manage your job and process.

- Useful knowledge
  - What is shell? (and what is the difference between shell, terminal and console?)
  - What is process?
  - What is job?
- `&`
- `Ctrl + Z`, `Ctrl + C`
- `jobs`
- `fg`
- `bg`
- `ps`
- `kill`
- `sleep`
- `nice`

## Useful knowledge
#### What is shell? (and difference between shell, terminal and console?)
So far I have been talking about shell and terminal. I think I might have confused you by mixing up the terms. I believe that most of people are confused about three terms: shell, terminal and console.

Shell is a command interpreter. Basically it is a program that repeats the following behavior: read command -> execute command -> wait for the next command -> repeat. So when you input a command, shell interprets what your command means. There is multiple kinds of shell. `bash` is widely used in linux systems and `zsh` is widely used in Macs.

Terminal is an actual machine that takes user's input and print the output. Long time ago in the early days of computers, computer was a huge machine(search for an image of mainframe computer). And users accessed computer through terminal which has a monitor and keyboard. Nowadays, terminal also means virtual terminal which is your 'terminal' program.

Console is a physical terminal.

And there is also a term, CLI which stands for command line interface. It means that program interacts with user via command line. On the contrary, GUI which stands for graphical user interface interacts with user via graphical things such as button, menubar, etc. 

#### What is process?
Program is a set of instructions for a computer to execute. So when you run your program, what your computer does is basically read and execute instruction repeatedly. In order to execute the program, your computer reads the program from file system (where file is stored on your hard drive or ssd) and load it onto the memory. Once it is loaded onto memory, CPU fetchs each instruction and executes it.

Process is an instance of program in exeuction. You can run the same program as many as you want at the same time like working on multiple windows of web browsers. Let's say you are using Chrome browser and you launched two windows of Chrome. It means that you have two process of Chrome, as though you just have one program of Chrome. In linux system, each process has its own unique number called `pid` as you will see soon in this tutorial.

#### What is job?
When you run a command in your shell, your shell keeps track of your command as a `job`. Each job has its own job number. You can manage your jobs by job number. You will see job soon in this tutorial.

## Commands for managing job and process
#### `sleep`
```bash
$ sleep 5
(...after 5 seconds)
$
```

`sleep` command pauses for specified seconds.

#### `&`
```bash
$ sleep 5 &
[1] 134055
(...immediately)
$ 
(...after 5 seconds)
[1]+  Done                    sleep 5
$
```

Appending `&` sign at the end of your command makes your command run in the background so that you can run another command immediately. Without `&` sign, your command runs in the foreground and your shell will be unavailable to interact until your command finishes. Running command in the background could be useful when you have to run long-running command.

#### `Ctrl + C`, `Ctrl + Z`
```bash
$ sleep 600
^C (Ctrl + C)
$

$ sleep 600
^Z (Ctrl + Z)
[1]+  Stopped                 sleep 600
$
```

In linux systems, processes can receive `signals` which tell processes to terminate or suspend. For example, if program goes into an infinite loop, we need some measures to stop that program. Signal serves for that purpose.

If you type `Ctrl + C`, you send a process `SIGINT` which interrupts a process. Most programs terminate when they receive `SIGINT` signal.

If you type `Ctrl + Z`, you send a process `SIGSTOP` which tells a process to stop. On the contrary to `Ctrl + C`, you can resume stopped process with `fg` or `bg` command. This means program pauses its execution. On the other hand, background job means that it execute in the background.

If you want to check out more information about linux signals, read the manual page with `man 7 signal`.

#### `jobs`
```bash
$ sleep 600
^Z (Ctrl + Z)
[1]+  Stopped                 sleep 600
$ jobs
[1]+  Stopped                 sleep 600
```

`jobs` shows jobs that your shell has run. You can think of running a command as a job. The number in the brackets is a job number.

#### `fg`
```bash
$ jobs
[1]+  Stopped                 sleep 600
$ fg %1
sleep 600
(...resume suspended job)
$ sleep 600 &
[2] 134395
$ fg %2
sleep 600
(...run background job as a foreground job)
```

`fg` can run suspended job or background job as a foreground job.

#### `bg`
```bash
$ jobs
[1]+  Stopped                 sleep 600
$ bg %1
[1]+ sleep 600 &
(...resume suspended job as a background job)
$
```

`bg` can run job as a background job.

#### `ps`
```bash
$ ps
    PID TTY          TIME CMD
  86656 pts/1    00:00:00 bash
 134378 pts/1    00:00:00 sleep
 134438 pts/1    00:00:00 ps
$
```

`ps` command shows processes. Each process has its own unique number `pid`. If you want to see all process in your system, you can add `aux` options.

```bash
$ ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0 245632 14404 ?        Ss   Jan04   0:55 /usr/lib/systemd/systemd --swi
root           2  0.0  0.0      0     0 ?        S    Jan04   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   Jan04   0:00 [rcu_gp]
root           4  0.0  0.0      0     0 ?        I<   Jan04   0:00 [rcu_par_gp]
root           6  0.0  0.0      0     0 ?        I<   Jan04   0:00 [kworker/0:0H-kblockd]
(...omitted)
$
```

#### `kill`
```bash
$ sleep 600 &
[1] 134505
$ ps
    PID TTY          TIME CMD
  86656 pts/1    00:00:00 bash
 134505 pts/1    00:00:00 sleep
 134506 pts/1    00:00:00 ps
$ kill 134505
$ ps
    PID TTY          TIME CMD
  86656 pts/1    00:00:00 bash
 134507 pts/1    00:00:00 ps
$
```

`kill` command can send signal to your process and terminate process. `kill` with no option sends a `SIGTERM` signal(signal number 15) which tell process to terminate. It gives your process some time to clean up before it terminates. If your process doesn't respond after receiving `SIGTERM` signal, you can send `SIGKILL` signal (signal number 9) with `kill -9 {PID}` to terminate your process immediately. `SIGTERM` is more desirable, but if it does not work, you can use `SIGKILL`.

#### `nice`

```
$ nice -n 5 ls
$ nice -n -19 ls
$ renice -n 10 -p {PID}
```

`nice` command set priority for running a command. Nice value can take range of -20 to 19, and less number means higher priority. Because linux systems runs lots of process compared to the number of CPU cores, each process have to share CPU. And processes with smaller nice number are more likely to have more CPU time.

You can re-adjust nice number with `renice` command.

## Wrap up
In this article, we have learned the following commands that manages jobs and processes. Try them on your terminal by yourself so that you can get used to them. If you are experiencing some troubles, you can check the manual by `man` command or you can leave a comment here. I'll see you in the next one. 

- Useful knowledge
  - What is shell? (and what is the difference between shell, terminal and console?)
  - What is process?
  - What is job?
- `&`
- `Ctrl + Z`, `Ctrl + C`
- `jobs`
- `fg`
- `bg`
- `ps`
- `kill`
- `sleep`
- `nice`
