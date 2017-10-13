---
layout: post
title: Shell常用资源
category: 资源
tags: Shell
keywords: Shell
description: 本文简介Shell并总结了常用的Shell命令
---

## Shell简介

Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。

Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。

Ken Thompson 的 sh 是第一种 Unix Shell，Windows Explorer 是一个典型的图形界面 Shell。


## Shell脚本

Shell 脚本（shell script），是一种为 shell 编写的脚本程序。

业界所说的 shell 通常都是指 shell 脚本，但大家要知道的是，shell 和 shell script 是两个不同的概念。

由于习惯的原因，简洁起见，本文出现的 "shell编程" 都是指 shell 脚本编程，不是指开发 shell 自身。

## Shell环境

Shell 编程跟 java、php 编程一样，只要有一个能编写代码的文本编辑器和一个能解释执行的脚本解释器就可以了。

Linux 的 Shell 种类众多，常见的有：
> Bourne Shell（/usr/bin/sh或/bin/sh）
> Bourne Again Shell（/bin/bash）
> C Shell（/usr/bin/csh）
> K Shell（/usr/bin/ksh）
> Shell for Root（/sbin/sh）
> ……
 

由于易用和免费，Bash 在日常工作中被广泛使用。同时，Bash 也是大多数Linux 系统默认的 Shell。

在一般情况下，人们并不区分 Bourne Shell 和 Bourne Again Shell，所以，像 **#!/bin/sh**，它同样也可以改为 **#!/bin/bash**。

`#!` 用来告诉系统其后路径所指定的程序即是解释此脚本文件的 Shell 程序。

## 常用Shell命令

### 1、ls：列出指定目录下的文件

ls最常用的参数有三个： -a，-l，-F。

ls –a

Linux上的文件以.开头的文件被系统视为隐藏文件，仅用`ls`命令是看不到他们的，而用`ls -a`除了显示一般文件名外，连隐藏文件也会显示出来。

ls –l

该参数显示更详细的文件信息，包括文件权限、创建时间、文件大小等。

ls –F

使用这个参数表示在文件的后面多添加表示文件类型的符号，例如*表示可执行，/表示目录，@表示连结文件，这都是因为使用了-F这个参数。但是现在基本上所有的Linux发行版本的ls都已经内建了-F参数，也就是说，不用输入这个参数，我们也能看到各种分辨符号。

![shell-ls](http://oxpypycim.bkt.clouddn.com/shell-ls.png)

### 2、cp：复制／改名文件

复制一个文件到另一目录：`cp 1.txt ../test2`

复制一个文件到本目录并改名：`cp 1.txt 2.txt`

复制一个文件夹a并改名为b：`cp -r a b`

### 3、mv：移动／重命名文件

将一个文件移动到另一个目录：`mv 1.txt ../test1`

将一个文件在本目录改名：`mv 1.txt 2.txt`

将一个文件一定到另一个目录并改名：`mv 1.txt ../test1/2.txt`

### 4、rm：删除指定文件／文件夹

rm命令用于删除文件，与dos下的`del`/`erase`命令相似，`rm`命令常用的参数有三个：-i，-r，-f。

–i ：系统在删除文件之前会先询问确认，用户回车之后，文件才会真的被删除。需要注意，linux下删除的文件是不能恢复的，删除之前一定要谨慎确认。

–r：该参数支持目录删除，功能和`rmdir`命令相似。

–f：和-i参数相反，-f表示强制删除

### 5、cd：切换用户工作目录

进入aaa目录：`cd aaa` 

cd 命令后不指定目录，会切换到当前用户的**home**目录

`cd ~` 作用同cd后不指定目录，切换到当前用户的home 目录

`cd -` 命令后跟一个减号，则会退回到切换前的目录

`cd ..` 返回到当前目录下的上一级目录

### 6、pwd：显示用户当前工作目录

![shell-pwd](http://oxpypycim.bkt.clouddn.com/shell-pwd.png)

### 7、mkdir／rmdir：创建／删除目录

两个命令都支持-p参数，对于`mkdir`命令若指定路径的父目录不存在则一并创建，对于`rmdir`命令则删除指定路径的所有层次目录，如果文件夹里有内容，则不能用rmdir命令

使用方法如下：

`mkdir -p 1/2/3`

`rmdir -p 1/2/3`

### 8、du、df：显示磁盘使用情况

`du`命令可以显示目前的目录所占用的磁盘空间，`df`命令可以显示目前磁盘剩余空间。

如果`du`命令不加任何参数，那么返回的是整个磁盘的使用情况，如果后面加了目录的话，就是这个目录在磁盘上的使用情况。

指定目录／查看指定目录的总大小：`du -hs` 

查看当前目录下的所有文件夹和文件的大小：`du -hs ./*`

这两个命令都支持-k，-m和-h参数，-k和-m类似，都表示显示单位，一个是k字节一个是兆字节，-h则表示human-readable，即友好可读的显示方式。

### 9、cat：显示或连结一般的ascii文本文件

cat是**concatenate**的简写，类似于dos下面的`type`命令。用法如下：

显示file1文件内容：`cat file1` 

依次显示file1, file2的内容：`cat file1 file2` 

把file1, file2的内容结合起来，再“重定向(>)”到file3文件中：`cat file1 file2 > file3` 

"**>**"是右重定向符，表示将左边命令结果当成右边命令的输入，注意：**如果右侧文件是一个已存在文件，其原有内容将会被清空，而变成左侧命令输出内容**。如果希望以追加方式写入，请改用"**>>**"重定向符。

如果"**>**"左边没有指定文件，如： `cat > file1`，将会等用户输入，输入完毕后再按`[Ctrl]+[c]`或`[Ctrl]+[d]`，就会将用户的输入内容写入file1。

### 10、echo：字符串输出

`echo`命令的使用频率不少于`ls`和`cat`，尤其是在shell脚本编写中。

语法：`echo [-ne][字符串]`

功能：echo会将输入的字符串送往标准输出，输出的字符串间以空白字符隔开， 并在最后加上换行符。

参数：

-n 显示字串时在最后自动换行

-e 支持以下格式的转义字符， -E 不支持以下格式的转义字符

/a 发出警告声;

/b 删除前一个字符;

/c 最后不加上换行符号;

/f 换行但光标仍旧停留在原来的位置;

/n 换行且光标移至行首;

/r 光标移至行首，但不换行;

/t 插入tab;

/v 与/f相同;

// 插入/字符;

/nnn 插入nnn(八进制)所代表的ASCII字符;

示例：


```Oracle@hjtest:~/hgd> echo "123" "456"

123 456

oracle@hjtest:~/hgd> echo "123/n456"

123/n456

oracle@hjtest:~/hgd> echo -e "123/n456"

123

456

oracle@hjtest:~/hgd> echo -E "123/n456"

123/n456

oracle@hjtest:~/hgd> echo -E "123///456"

123//456

oracle@hjtest:~/hgd> echo -e "123///456"

123/456

oracle@hjtest:~/hgd> echo -e "123/100456"

123@456
```

注意事项：

在Linux使用的bash下，单引号’’和双引号是有区别的，单引号忽略所有的转义，双引号不会忽略以下特殊字符：

Dollar signs ($)，Back quotes (`)，Backslashes (/)，Excalmatory mark(!)

示例如下：


```oracle@hjtest:~> echo "`TEST`"

-bash: TEST: command not found

oracle@hjtest:~> echo '`TEST`'

`TEST`

oracle@hjtest:~> echo "$TEST"

oracle@hjtest:~> echo '$TEST'

$TEST

oracle@hjtest:~> echo "//TEST"

/TEST

oracle@hjtest:~> echo '//TEST'

//TEST

oracle@hjtest:~> echo "Hello!"

echo "Hello"

Hello

oracle@hjtest:~> echo 'Hello!'

Hello!
```


