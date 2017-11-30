---
layout: post
title: Mac下编译TensorFlow
category: 技术
tags: TensorFlow
description: 在MacBook Pro上编译TensorFlow
---

#### pip安装TensorFlow的问题

随着TensorFlow功能越来越强大，好多算法开发人员都在尝试使用，并通过pip来安装，不过这种方法安装的都是已经编译好的版本，并没有对本地环境做任何优化。因此调试过程中经常出现下面这种警告，大致就是说你机器支持加速运算的指令，但由于编译时没有启用，所以你用不了（微笑脸）。

```python
2017-11-30 20:47:52.933324: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.2 AVX AVX2 FMA
```

当然了，你可以选择视为不见，就像下面这种方法：

```python
import os
os.environ['TF_CPP_MIN_LOG_LEVEL']='2'
import tensorflow as tf
```

也可以选择花20分钟编译一个健全的TensorFlow。因为本来新MBP没有N卡已经够慢了，再阉割这些加速指令，就更残废了。

#### 环境

我的环境是**macOS 10.12.6**，**Anaconda3**，**python3.6**。

#### 安装bazel

> bazel是google推出的一款工程编译工具，并且已经将其开源。功能很强大，但也有点复杂，感兴趣的同学可以自行学习一下。

首先需要安装bazel，使用下面的命令：

```
brew install bazel
```

![bazel_install_1](http://oxpypycim.bkt.clouddn.com/bazel_install_1.png)


当输入`bazel version`出现以下内容则表示安装成功：

![bazel_install_2](http://oxpypycim.bkt.clouddn.com/bazel_install_2.png)

#### 安装TensorFlow

##### 配置

-------

`pip list`来查看当前所有的python包列表，然后`pip uninstall tensorflow`卸载掉当前的TensorFlow。

接着去GitHub上clone源码，之后进入目录执行`./configure`

![tf_config](http://oxpypycim.bkt.clouddn.com/tf_config.png)

之后就一路回车，反正MBP这些**都。不。支。持。。。**

直到看到`Configuration finished`表明配置完成了。

##### 编译

-------

执行下面代码开始开始编译：


```
bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package
```

大概20分钟后。。。

![tf_config_finish](http://oxpypycim.bkt.clouddn.com/tf_config_finish.png)

看到上面内容说明编译完成，同时发现文件夹中会生成一堆binary文件和软连接。

![tf_dir](http://oxpypycim.bkt.clouddn.com/tf_dir.png)

接着我们就可以通过下面命令来生成wheel文件了：

```
bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
```

出现下面提示说明wheel生成成功。

```
~/Desktop/tensorflow
2017年11月30日 星期四 22时51分55秒 CST : === Output wheel file is in: /tmp/tensorflow_pkg
```

##### 安装

-------

下面来安装生成的wheel，还是用pip来安装：

```
pip install tensorflow-1.4.0-cp36-cp36m-macosx_10_7_x86_64.whl
```

安装成功提示：

![tf_install_wheel](http://oxpypycim.bkt.clouddn.com/tf_install_wheel.png)

##### 测试

-------

好了，现在来测试一下：

![tf_conf_done](http://oxpypycim.bkt.clouddn.com/tf_conf_done.png)

看不到之前的问题了，说明TensorFlow已经工作正常。

如果遇到下面的问题：

```
Failed to load the native TensorFlow runtime
```

不要在TensorFlow编译目录中运行python，cd到其他历经就行了。

