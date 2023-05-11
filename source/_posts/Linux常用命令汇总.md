---
title: Linux常用命令行汇总
tags:
  - bash
categories:
  - 工具
abbrlink: '96574460'
date: 2022-11-20 12:49:42
---

Linux命令一般为位于/usr/bin目录下，以下汇总工作中接触比较多的命令。
<!-- more-->

## 基础命令
---

### 查看文件和目录

{% markmap 400px %}
- [ls](#ls)
  - List directory contents.
- [cat](#cat)
  - Print and concatenate files.
- [less](#less)
  - Open a file for interactive reading, allowing scrolling and search.
- [more](#more)
  - Open a file for interactive reading, allowing scrolling and search.
- [head](#head)
  - Output the first part of files.
- [tail](#tail)
  - Display the last part of a file.
- [file](#file)
  - Determine file type.
- [find](#find)
  - Find files or directories under the given directory tree, recursively.
{% endmarkmap %}

### 操作文件和目录

{% markmap 400px %}
- [touch](#touch)
  - Change a file access and modification times (atime, mtime).
- [mkdir](#mkdir)
  - Creates a directory.
- [cp](#cp)
  - Copy files and directories.
- [ln](#ln)
  - Creates links to files and directories.
- [mv](#mv)
  - Move or rename files and directories.
- [rm](#rm)
  - Remove files or directories.
{% endmarkmap %}

### 管理文件或目录权限

{% markmap 400px %}
- [chmod](#chmod)
  - Change the access permissions of a file or directory.
- [chown](#chown)
  - Change user and group ownership of files and directories.
- [chgrp](#chgrp)
  - Change group ownership of files and directories.
{% endmarkmap %}

### 文本处理

{% markmap 400px %}
- [sort](#sort)
  - Sort lines of text files.
- [uniq](#uniq)
  - Output the unique lines from the given input or file.
- [tr](#tr)
  - Translate characters: run replacements based on single characters and character sets.
- [grep](#grep)
  - Find patterns in files using regular expressions.
- [diff](#diff)
  - Compare files and directories.
- [wc](#wc)
  - Count lines, words, and bytes.
- [awk](#awk)
  - A versatile programming language for working on files.
- [sed](#sed)
  - Edit text in a scriptable manner.
{% endmarkmap %}

### 其他常用命令

{% markmap 400px %}
- [hostname](#hostname)
  - Show or set the system's host name.
- [who](#who)
  - Display who is logged in and related data (processes, boot time).
- [uptime](#uptime)
  - Tell how long the system has been running and other information.
- [uname](#uname)
  - Print details about the current machine and the operating system running on it.
- [date](#date)
  - Set or display the system date.
- [id](#id)
  - Display current user and group identity.
{% endmarkmap %}

## 进阶命令
---

### 文件处理和归档

{% markmap 400px %}
- [paste](#paste)
  - Merge lines of files.
- [dd](#dd)
  - Convert and copy a file.
- [gzip](#gzip)
  - Compress/uncompress files with gzip compression (LZ77).
- [bzip2](#bzip2)
  - A block-sorting file compressor.
- [gunzip](#gunzip)
  - Extract file(s) from a gzip (.gz) archive.
- [tar](#tar)
  - Archiving utility.
{% endmarkmap %}

### 检测和管理磁盘

{% markmap 400px %}
- [mount](#mount)
  - Provides access to an entire filesystem in one directory.
- [umount](#umount)
  - Unlink a filesystem from its mount point, making it no longer accessible.
- [df](#df)
  - Gives an overview of the filesystem disk space usage.
- [du](#du)
  - Disk usage: estimate and summarize file and directory space usage.
{% endmarkmap %}

### 后台执行命令

{% markmap 400px %}
- [crontab](#crontab)
  - Schedule cron jobs to run on a time interval for the current user.
- [at](#at)
  - Executes commands at a specified time.
- [nohup](#nohup)
  - Allows for a process to live when the terminal gets killed.
{% endmarkmap %}

## 参考

【1】tldr工具简略查询命令。链接：[tldr](https://pypi.org/project/tldr/)
【2】人性化命令查询网址：https://linux.cmsblogs.cn/
【3】manned官网：https://manned.org/