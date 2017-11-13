---
layout: post
title: Win10+CUDA8+cuDNN6+tensorflow1.5+keras2.0环境配置记
category: 技术
tags: 环境配置
description: 在windows10系统下配置机器学习环境
---

> 《序》部分为日常碎碎念，《第一次尝试》是失败的，踩了许多坑。着急的看官老爷们可直接跳到《**第一次尝试》tensorflow安装部分**和**《第二次尝试》**章节。

## 序

> 2017年11月12日早上10点，揉了揉眼睛爬起床，看着窗外难得的“北京蓝”心想今天真是个出去high的好日子（燃鹅。。。最近着凉了。。）。好吧，那总得找点事儿干吧！正巧前段时间买的游戏本还没装啥游戏，那好，就把GPU的“第一次”献给机器学习吧！

> 当然，考虑到游戏本的性能远不及我平时工作用的服务器，所以就不对它下手太狠了。。。更何况我还要靠它“吃鸡”呢，于是**Win10+tensorflow+keras**的环境出现在了我的脑海中。

-------

## 一、机器情况

我手头的游戏本型号是`微星GL62M`，官网的参数请看下图：

![电脑配置](http://oxpypycim.bkt.clouddn.com/电脑配置.png)

买来后我加了一块256G的SSD，并重做的Win10专业版系统，其他参数都不变。好了，情况就是这样了，下面开始配置！

-------

## 二、第一次尝试

首先确保Nvidia的显卡驱动有正确安装，我这里用的是是官方最新的驱动（**版本号388.13**）。

### CUDA安装

打开**CUDA**的[下载页面](https://developer.nvidia.com/cuda-downloads)，选择好电脑配置并下载（**注意**：强烈推荐选用**`exe（local）`**来本地安装CUDA，因为在线版的巨慢，别问我为什么知道），等待下载完成。

安装过程比较简单，但开始时CUDA会检测当前的驱动版本，如果驱动版本高于CUDA的自带版本，就会报下面这种错误。

![cuda兼容性问题](http://oxpypycim.bkt.clouddn.com/cuda兼容性问题.PNG)

不用管它，直接选`继续`。等待CUDA安装完，然后在系统变量`Path`中添加cuda的`bin`和`lib／x64`这两个路径。

### cuDNN安装

接着我们装**cuDNN**，网址戳[这里](https://developer.nvidia.com/rdp/cudnn-download)，下载之前需要先注册成为Nvidia的developer，登录后才可以下载。

之后将解压出来的`cuda`文件夹里的内容替换掉`CUDA`安装路径对应文件就行，安装完成！

### Anaconda安装

下面我们开始安装python环境，估计没有不知道**Anaconda**的python开发朋友吧（不知道的自己百度去），我们就用它来配置python环境。

但是Anaconda的服务器不在国内，下载巨慢！好在有清华的[镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)，新版的Anaconda会默认使用**python3.6**，安装时`“Add Anaconda to my PATH environment variable”`这里可以不选，用到时直接去安装目录下执行命令。等着安装完成。

![anaconda安装1](http://oxpypycim.bkt.clouddn.com/anaconda安装1.jpg)

### TensorFlow安装

网上有好多教程说直接`pip install tensorflow-gpu`，这种方法可行。我是按照tensorflow github上的方法、对于win10用户来说需要右键`开始`菜单，打开`Windows PowerShell`，`cd`到**yourpath／Anaconda／Scripts**中，执行：

```
.\pip.exe install tf-nightly-gpu
```
![tf_install1](http://oxpypycim.bkt.clouddn.com/tf_install1.PNG)

![winPowerShell](http://oxpypycim.bkt.clouddn.com/winPowerShell.PNG)

但在这里我遇到了下面的错误：

```
TypeError: parse() got an unexpected keyword argument 'transport_encoding'
```

貌似encoding出了问题，解决方法是执行：

```
.\conda.exe install -c anaconda html5lib
```

-------

500年后。。。还没装完？？？赶紧换清华镜像站的源，方法如下：

```
.\conda.exe config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/

.\conda.exe config --set show_channel_urls yes
```

重新输入命令安装上面的命令，等待安装完成。

### pyCharm安装

**pyCharm**我认为是windows系统下一款非常优秀的python IDE，功能覆盖全面，谁用谁知道！

安装后新建工程，**pyCharm**会自动选择**anaconda**下的python编译器。

![pycharm_newproject](http://oxpypycim.bkt.clouddn.com/pycharm_newproject.PNG)

好了，现在跑段代码试一下！

```python
import tensorflow as tf
tip = tf.constant('looks fun!')
sess = tf.Session()
print(sess.run(tip))
```

然后，遇到了各种找不到cuda库的`error`。意识到，原来**tensorflow1.5.0**还是没有提供**CUDA 9 - cuDNN 7**的支持！！！github上关于这个的`issue #12052`已经挂了好久了好伐。。。

好吧，我的第一次环境配置以失败告终。

-------

## 三、第二次尝试

找到了root cause，自然也就找到了解决方法。百度了一下，各路大神少侠也遇到过相似的问题，更有大神直接修改tensorflow源码并重新编译，想折腾的看官老爷出门左转去搜`CUDA 9.0 + cuDNN 7.0 + Tensorflow源码编译`。

不过我并不想折腾。。。

我的解决方案是：不动**tensorflow**，直接卸载**CUDA 9**，删除之前配好的环境变量，装回`CUDA 8 - cuDNN 6` ！

### CUDA 8 安装

由于**CUDA**官网上默认安装最新版本，所以我需要找到旧的release版，比如[CUDA Toolkit 8.0 GA2](https://developer.nvidia.com/cuda-80-ga2-download-archive)。

之后依然是选择系统对应的版本，下载并安装，步骤和之前一样。

### cuDNN 6 安装

按照这个思路我需要再找到**cuDNN**的旧版本，打开官网，啊咧！居然旧版的最高版本只是cuDNN 5。。。

后来终于在网盘里找到了cuDNN 6，目前已转到我的网盘，需要的自取。

```
链接：http://pan.baidu.com/s/1miacQI8 密码：n8q0
```

安装方法和之前的一样。记得要配好环境变量。

### 尝试

满怀欣喜地装完了**CUDA**和**cuDNN**，打开**pyCharm**，之前的代码测试通过！不过总觉得不把GPU跑一下心里不爽，于是希望跑一个复杂一些的模型。

为了快速建模，我选择安装**keras**。安装方法很简单，在之前`Anaconda3／Scripts`下：

```
.\pip.exe install keras
```

安装好后，回到**pyCharm**中，像下面这样写个复杂点的mnist模型：


```python
from __future__ import print_function
import numpy as np
np.random.seed(6666)

from keras.models import Sequential
from keras.layers import Dense, Activation, Flatten, BatchNormalization
from keras.layers import Convolution2D, MaxPooling2D
from keras.utils import np_utils

batch_size = 256
nb_classes = 10
nb_epoch = 20

img_rows, img_cols = 28, 28
nb_filters = 64
pool_size = (2,2)
kernel_size = (3,3)
f = np.load('mnist.npz')
X_train, y_train = f['x_train'], f['y_train']
X_test, y_test = f['x_test'], f['y_test']
f.close()

X_train = X_train.reshape(X_train.shape[0], img_rows, img_cols, 1)
X_test = X_test.reshape(X_test.shape[0], img_rows, img_cols, 1)
input_shape = (img_rows, img_cols, 1)
X_train = X_train.astype('float32')
X_test = X_test.astype('float32')

Y_train = np_utils.to_categorical(y_train, nb_classes)
Y_test = np_utils.to_categorical(y_test, nb_classes)

model = Sequential()

model.add(Convolution2D(nb_filters, kernel_size, padding='same', input_shape=input_shape, name='conv1'))
model.add(BatchNormalization(name='bn1'))
model.add(Activation('relu'))
model.add(BatchNormalization(name='bn2'))

model.add(Convolution2D(nb_filters, kernel_size, padding='same', input_shape=input_shape, name='conv2'))
model.add(BatchNormalization(name='bn3'))
model.add(Activation('relu'))
model.add(BatchNormalization(name='bn4'))
model.add(MaxPooling2D(pool_size, name='pool1'))

model.add(Flatten(name='flat'))

model.add(Dense(64, name='fc1'))
model.add(BatchNormalization(name='bn5'))
model.add(Activation('relu'))
model.add(Dense(64, name='fc2'))
model.add(BatchNormalization(name='bn6'))
model.add(Activation('relu'))
model.add(Dense(10, name='fc3'))
model.add(BatchNormalization(name='bn7'))
model.add(Activation('softmax'))

model.summary()

model.compile(loss='categorical_crossentropy',
              optimizer='adam',
              metrics=['acc'])
              
model.fit(X_train, Y_train, batch_size=batch_size, epochs=nb_epoch,
          verbose=1, validation_split=0.3, validation_data=(X_test, Y_test))
          
score = model.evaluate(X_test, Y_test, verbose=0)

model.save('cnn_model.h5')

print('Test loss: {}'.format(score[0]))
print('Test accuracy: {}'.format(score[1]))
```

OK，run起来了，大功告成！关于显卡的状态，可以去`C:\Program Files\NVIDIA Corporation\NVSMI`中，执行`.\nvidia-smi.exe -l`来查看。

![nvidia_smi](http://oxpypycim.bkt.clouddn.com/nvidia_smi.PNG)

运行结果如下：

![mnist_test](http://oxpypycim.bkt.clouddn.com/mnist_test.PNG)

-------

## 总结

整个环境配置下来该踩的坑也差不多都踩了，配置过程充满surprise，不过最终成功运行得到结果还是很有成就感的。

ps：配环境之前一定要先弄清楚软件的版本要求，可以少走许多弯路。

