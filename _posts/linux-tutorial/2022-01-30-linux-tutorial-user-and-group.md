---
layout: post
title: Linux Tutorial (3) - User and Group
description: Linux commands to deal with user and group
author: Seungwoo Jo
# last_modified_at: 2022-01-14 22:10:00 +0900
# math: false
tags: linux tutorial
category: Linux Tutorial
comments: true
---

## Overview
In this tutorial, I am going to talk about following commands which can help you manage users and groups in your linux system.

- About user and group
- `whoami`, `groups`, `id`
- `passwd`
- `w`, `last`, `lastb`
- `useradd`, `userdel`
- `groupadd`, `groupdel`, `usermod`

## About user and group
Linux is built to be used simultaneously by multiple users. Multiple user have to be managed with different level of permissions, otherwise host computer would end up in a mayhem.

There are two types of users: superuser whose name is `root` and regular users which can be named as anything. Superuser can do everything, so access to `root` user should be restricted only for administrators. Regular user has their own permissions to read/write/execute files. Users have their own home directory which is located at `/home/{username}` and in case of `root` user `/root`. If you use `ls -l` command, you can check the permissions for files. r, w and x each stands for permission to read, write and execute. First sequence means permission for owner user, second is for owner group and last sequence menas permission for everyone. So in the below case, user `spicyjo` has read/write permission to `newtodo.txt` while everyone else has read permission to the file. `newtodo.txt` file belongs to user `spicyjo` and group `spicyjo`.

```bash
[spicyjo@spicyhost-spicydomain ~]$ ls -l
total 4
-rw-rw-r--. 1 spicyjo spicyjo 46 Jan 13 10:29 newtodo.txt
-rw-rw-r--. 1 spicyjo spicyjo  0 Jan 13 10:32 test.txt
```

User can belong to groups. One user can belong to multiple groups. When user is created for the first time, the user is associated to one group which is called **primary group**. By default, primary group is created with group name identical to user name (e.g. there is user `spicyjo` which belongs to group `spicyjo`). After the user is created, it can be added to other groups which is called **secondary group**.

Each user and group has its own identifier number which is called **uid** and **gid**.

User information is kept in file `/etc/passwd`. Users' password are stored in `/etc/shadow` in hashed format. Likewise group information is kept in file `/etc/group` and `/etc/gshadow`.


## `whoami`, `groups`, `id`
```bash
$ whoami
spicyjo
$ groups
spicyjo
$ id
uid=1001(spicyjo) gid=1001(spicyjo) groups=1001(spicyjo) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```

`whoami` prints current user id, and `groups` prints current group id. `id` command prints more information about current user and group such as uid and gid.

## `passwd`
```bash
$ passwd
Changing password for user spicyjo.
Current password:
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
```

`passwd` command changes current user's password. It does not display the password you type for security. If you are logged in as a superuser, you can also change other user's password by `passwd {username}`.

## `w`, `last`, `lastb`
```bash
$ w
 09:10:16 up 25 days, 14:02,  2 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
spicyjo  pts/1    172.30.1.42      08:22    0.00s  0.07s  0.00s sshd: spicyjo [priv]
```

`w` command shows currently logged-in users' information. 

```bash
$ last
spicyjo   pts/1        172.30.1.42      Sun Jan 30 08:22   still logged in
spicyjo   pts/1        172.30.1.42      Tue Jan 11 09:52 - 05:20 (4+19:28)
reboot   system boot  4.17.0-240.el8.x Tue May  4 09:12 - 13:22  (04:10)

wtmp begins Tue May  4 09:12:14 2021
```

```bash
[root@spicyhost-spicydomain root]# lastb
root     pts/1                         Sun Jan 30 08:24 - 08:24  (00:00)

btmp begins Sun Jan 30 08:23:02 2022
```

`last` command shows login log and `lastb` shows login log of superuser. In order to run `lastb`, you have to be logged in as a superuser.

## `useradd`, `userdel`
```bash
[root@spicyhost-spicydomain root]# useradd drummer
[root@spicyhost-spicydomain root]# ls /home/
drummer  spicyjo  guitarman
[root@spicyhost-spicydomain root]# userdel drummer
[root@spicyhost-spicydomain root]#
```

`useradd` command creates user and `userdel` command delete user. You have to be logged in as a superuser in order to run these commands.

## `groupadd`, `groupdel`, `usermod`
```bash
[root@spicyhost-spicydomain root]# groupadd marchband
[root@spicyhost-spicydomain root]# usermod -a -G marchband spicyjo
[root@spicyhost-spicydomain root]# groups spicyjo
spicyjo : spicyjo marchband
[root@spicyhost-spicydomain root]# groupdel marchband
[root@spicyhost-spicydomain root]# groups spicyjo
spicyjo : spicyjo
```

`groupadd` command creates group and `groupdel` command delete group. You have to be logged in as a superuser in order to run these commands. `usermod` command modifies a user account. `usermod -a -G {groupname} {username}` adds the user to specified group.

## Wrap up
In this article, we have learned the following commands that manages users and groups. Try them on your terminal by yourself so that you can get used to them. If you are experiencing some troubles, you can check the manual by `man` command or you can leave a comment here. I'll see you in the next tutorial.

- About user and group
- `whoami`, `groups`, `id`
- `passwd`
- `w`, `last`, `lastb`
- `useradd`, `userdel`
- `groupadd`, `groupdel`, `usermod`
