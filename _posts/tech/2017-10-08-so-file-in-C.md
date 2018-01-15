---
layout: post
title: .so文件是什么
category: 技术
tags: C
description: 简要介绍.so文件
---

> .so文件是Linux下的程序函数库,即编译好的可以供其他程序使用的代码和数据

### Linux下何谓.so文件：

1. 用过windows的同学应该都知道 **.dll**文件吧, 这二者有什么共通之处呢,其实 **.so**文件就跟**.dll**文件差不多；
2. 一般来说**.so**文件就是常说的动态链接库, 都是C或C++编译出来的。与**Java**比较就是：它通常是用的Class文件（字节码）；
3. Linux下的**.so**文件时不能直接运行的,一般来讲,**.so**文件称为共享库；
4. 那么.so文件是怎么用的呢？

**例如：**

#### 1、动态库的编译

这里有一个头文件：`so_test.h`，三个.c文件：`test_a.c`、`test_b.c`、`test_c.c`，我们将这几个文件编译成一个动态库：`libtest.so`。

命令：

```shell
$ gcc test_a.c test_b.c test_c.c -fPIC -shared -o libtest.so
```

其中：

`-shared`： 该选项指定生成动态连接库（让连接器生成T类型的导出符号表，有时候也生成弱连接W类型的导出符号），**不用该标志外部程序无法连接**。相当于一个可执行文件。

`-fPIC`：表示编译为位置独立的代码，**不用此选项的话编译后的代码是位置相关的**，所以动态载入时是通过代码拷贝的方式来满足不同进程的需要，而不能达到真正代码段共享的目的。

#### 2、动态库的链接

这里有个程序源文件 test.c 与动态库 libtest.so 链接生成执行文件 test：

命令：

```shell
$ gcc test.c -L. -ltest -o test
```

（注：测试是否动态连接，如果列出`libtest.so`，那么应该是连接正常了）

其中：

`-L.`：表示要连接的库在当前目录中。
`-ltest`：编译器查找动态连接库时有隐含的命名规则，即在给出的名字前面加上lib，后面加上.so来确定库的名称。

命令：


```shell
$ ldd test
```

（注：执行test，可以看到它是如何调用动态库中的函数的。）

