---
layout: post
title: Sox工具的用法
category: 工具
tags: sox， Linux
description: Linux下sox工具的用法
---

## Sox是什么

Sox是最为著名的**Open Source**声音文件格式转换工具。已经被广泛移植到`Dos`、`windows`、`Mac OS`、`Sun`、`Next`、`Unix`、`Linux`等多个操作系统平台。

Sox项目是由**Lance Norskog**创立的，后来被众多的开发者逐步完善，现在已经能够支持很多种声音文件格式和声音处理效果。基本上常见的声音格式都能够支持。更加有用的是，Sox能够进行声音滤波、采样频率转换，这对那些从事声讯平台开发或维护的朋友非常有用。当然，Sox里面也包括一些DSP算法，功能十分强大。

## Sox用法

Sox的使用方法简单，主要设置如下：

![sox3](http://oxpypycim.bkt.clouddn.com/sox3.JPG)

我们可以通过`play`命令来播放音频，支持如**wav**，**mp3**，**pcm**等常见音频格式。

![sox1](http://oxpypycim.bkt.clouddn.com/sox1.JPG)

通过`soxi *.wav`命令来查看音频信息，如采样率、位宽等。

![sox2](http://oxpypycim.bkt.clouddn.com/sox2.JPG)

同时，我们可以转换音频的采样率，通道数,量化位数，文件类型等，并输出新音频。

    sox -r 16000 -c 1 -b 16 -t wav input.wav output.wav

Sox也适用于切割音频，只需使用如下命令：

    sox input.wav output.wav trim 1.0 3.0

其中，1.0表示从哪个时刻开始切割，3.0表示持续时间，所以上面的命令是切割input.wav文件的第1.0秒到第4.0秒，并输出到output.wav中。

对于多段语音切割，可循环调用该命令，如：

    for index in {1..100}; do
        sox input.wav output_${index}.wav trim ${index} 1.5
    done

将input.wav切分成100份，每份持续1.5秒。

