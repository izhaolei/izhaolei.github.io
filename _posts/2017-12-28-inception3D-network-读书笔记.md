---
title: Quo Vadis, Action Recognition? A New Model and the Kinetics Dataset(I3D) 论文笔记
mathjax: true
type: categories
categories: paper-note
---

​     在文中作者主要介绍了**Kinetics**行为识别数据库，同时作者提出了一种新的3D卷积网络构建与权值初始化方式，作者以膨胀**($Inflated$)**的方式构建3D网络**($I3D$)** ，实验表明：根据2D网络的结构和权值进行膨胀能获得和很好的性能，这篇文章中提到利用使用在**ImageNet**上训练的2D网络初始化膨胀后的3D网络，然后再Kinetics数据集上训练，最后在**UCF-101**与**HMDB-52**数据集上finetune可以分别达到**80.7%**与**98.0%**的正确率。**[论文链接](https://arxiv.org/abs/1705.07750)**

#### Inflated 3D ConvNets

​     今年来图像分类网络结构已经十分成熟了，这些2D网络结构广泛的运用于计算机视觉任务中，为了将这些成熟的网络结构运用到视频任务中，作者将2D网络进行膨胀，即，对于网络中的**filter**与**pooling kernels**进行膨胀，举例来说：对于简单的filter kernels 一般是方形$N\times N$ 的，将这样的kernel扩张为$N\times N\times N$ 。

​    一般来说，直接3D网络由于kernel维度激增，造成需要优化的参数增加，使得优化网络变得更加困难，也更加容易过拟合，作者使用在ImageNet上训练过的2D网络参数来初始化3D网络。对于由$N\times N$ 拓展来的$N\times N\times N$ kernel，作者将$N\times N$ kernel的参数简单的拷贝$N$ 份，然后对参数除以$\frac{1}{N}$ 。

#### two stream Inflated 3D ConvNets 

 ![](https://github.com/izhaolei/images/blob/master/i3dv1.png?raw=true)

two stream I3D结构如上图，在训练过程中作者使用$224\times 224$ 的分辨率进行训练，同时增大了同时输入的量达到了64 rames。网络在UCF-101上达到了很高的性能，及时不和光流ensemble也能达到不错的结果(95.6%)。

