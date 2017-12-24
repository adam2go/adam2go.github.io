---
layout: post
title: 树莓派扩充SWAP的正确姿势
category: 技术
tags: 树莓派
description: 正确扩充树莓派SWAP的方法
---

>树莓派是一款小巧、便携、而又功能强大的个人电脑，目前全球出货量位居第三，当然了，第一是WIN平台、第二是MAC平台。
>但是，目前最新的树莓派3B也仅有1GB的运存，随便运行个大程序，cache分分钟占满内存，所以需要通过SWAP来决解内存不足的短板。

## 旧方法

百度“树莓派 swap”关键词会出来很多树莓派扩充swap的方法，大多教程是这样的：


```shell
cd /var
sudo swapoff /var/swapfile
sudo dd if=/dev/zero of=swapfile bs=1M count=1024
sudo mkswap /var/swapfile
sudo swapon /var/swapfile
sudo nano /etc/fstab
```

可是，我试了N遍，这方法根本行不通，有毒吧！～～


## 正确方法

直接用`sudo`，修改`/etc/dphys-swapfile`文件的**CONF_SWAPSIZE**值。原始值是100M，把它改大点，比如2048M，就行了。

而且我发现，改了这个以后，重启也不影响SWAP的改动。

这才是树莓派改SWAP的正确姿势～


