---
title: Linux-shell 命令行
tags:
  - bash
categories:
  - 工具
abbrlink: '96574460'
date: 2022-11-20 12:49:42
---


### 介绍
---  
{% blockquote 菜鸟教程 https://www.runoob.com/linux/linux-shell.html Shell 教程 %}
Shell 是一个用C编写的应用程序，它是供用户使用Linux的桥梁。
{% endblockquote %}

<!-- more-->

Shell 既是一种命令语言，又是一种程序设计语言。Shell脚本是一种使用shell语言编写的脚本程序。Unix/Linux上常见的Shell脚本解释器有bash、sh、csh、ksh等，习惯上把它们称作一种Shell。我们常说有多少种Shell，其实说的是Shell脚本解释器。

### 目录 & 文件
---
{% markmap 400px %}
- [ls](#ls)
  - List directory contents.
- [pwd](#pwd)
  - Print name of current/working directory.
- [cd](#cd)
  - Change the current working directory.
- [file](#file)
  - Determine file type.
- [less](#less)
  - Open a file for interactive reading, allowing scrolling and search.
{% endmarkmap %}


{% note primary no-icon %}
#### ls
- list directory contents
{% endnote %}
{% codeblock "ls 用法" lang:bash https://www.gnu.org/software/coreutils/ls "官方链接"%}
# List files one per line:
  ls -1
# List all files, including hidden files:
  ls -a
# List all files, with trailing `/` added to directory names:
  ls -F
# Long format list (permissions, ownership, size, and modification date) of all files:
  ls -la
# Long format list with size displayed using human-readable units (KiB, MiB, GiB):
  ls -lh
# Long format list sorted by size (descending):
  ls -lS
# Long format list of all files, sorted by modification date (oldest first):
  ls -ltr
# Only list directories:
  ls -d */
{% endcodeblock %}


{% note success no-icon %}
#### pwd
- Print name of current/working directory.
{% endnote %}
{% codeblock "pwd 用法" lang:bash https://www.gnu.org/software/coreutils/pwd "官方链接"%}
# Print the current directory:
  pwd
# Print the current directory, and resolve all symlinks (i.e. show the "physical" path):
  pwd --physical
# Print the current logical directory:
  pwd --logical
{% endcodeblock %}


{% note info no-icon %}
#### cd
- Change the current working directory.
{% endnote %}
{% codeblock "cd 用法" lang:bash https://manned.org/cd "官方链接"%}
# Go to the specified directory:
  cd [path/to/directory]
# Go up to the parent of the current directory:
  cd ..
# Go to the home directory of the current user:
  cd
# Go to the home directory of the specified user:
  cd ~[username]
# Go to the previously chosen directory:
  cd -
{% endcodeblock %}


{% note warning no-icon %}
#### file
- Determine file type.
{% endnote %}
{% codeblock "file 用法" lang:bash https://manned.org/file "官方链接"%}
# Give a description of the type of the specified file. Works fine for files with no file extension:
  file [filename]
# Look inside a zipped file and determine the file type(s) inside:
  file -z [foo.zip]
# Allow file to work with special or device files:
  file -s [filename]
# Don't stop at first file type match; keep going until the end of the file:
  file -k [filename]
# Determine the mime encoding type of a file:
  file -i [filename]
{% endcodeblock %}


{% note danger no-icon %}
#### less
- Open a file for interactive reading, allowing scrolling and search.
{% endnote %}
{% codeblock "less 用法" lang:bash https://greenwoodsoftware.com/less/ "官方链接"%}
# Open a file:
  less [source_file]
# Page down / up:
  [Space] (down), b (up)
# Go to end / start of file:
  G (end), g (start)
# Forward search for a string (press `n`/`N` to go to next/previous match):
  /[something]
# Backward search for a string (press `n`/`N` to go to next/previous match):
  ?[something]
# Follow the output of the currently opened file:
  F
# Open the current file in an editor:
  v
# Exit:
  q
{% endcodeblock %}
