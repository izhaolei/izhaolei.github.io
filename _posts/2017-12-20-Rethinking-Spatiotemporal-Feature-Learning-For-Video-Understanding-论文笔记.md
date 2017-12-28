---
title: Rethinking Spatiotemporal Feature Learning For Video Understanding 论文笔记
mathjax: true
type: categories
categories: paper-note
---

在这篇文章中，作者系统的谈久了3D 卷积网络对于行为识别的作用。从目前的state-of-the-art工作[I3D](https://arxiv.org/abs/1705.07750) 开始，研究了3D卷积在神经网络不同位置所起的作用，同时作者提出了gated spatiotemporal-separable 3D卷积<font color=blue >[论文链接](https://arxiv.org/abs/1712.04851)</font>。

#### 文章主要出发点

文章期限提出三个问题并以此为出发点：

> 1、 在网络中是否需要全部都是3D卷积结构
>
> 2、 在时间(time)与空间(space)两个维度同时卷积(3D卷积)是否有必要
>
> 3、 在探究上两个问题的基础上怎样改进网络结构，提升识别性能

##### 第一个问题

作者为了回答第一个问题构建了一组网络，分别如下图：

![](https://github.com/izhaolei/images/blob/master/I3Ds.JPG?raw=true)

其中$3D\ Inc.$与$2D\ Inc.$分别代表3D Inception block 与2D Inception block，结构如下图：

![](https://github.com/izhaolei/images/blob/master/3d-2d-inception.JPG?raw=true)

作者构建了4中结构，然后分别探究它们的性能表现。其中I3D结构性能优于I2D：

| Model | Normal(%) | Shuffle(%) | Reversed(%) |
| :---: | :-------: | :--------: | :---------: |
|  I3D  |   71.66   |   45.37    |    71.54    |
|  I2D  |   67.00   |   67.52    |    67.23    |

可以看出，3D结构优于考虑了时间轴 ，对于数据的顺序结构更加敏感，而2D结构在顺序结构发生变化时鲁棒性更强。

同时作者发现，将I2D结构的顶部换成3D结构的性能比将底部换为3D结构更好，同时前者的计算量更小。

##### 第二个问题

仿照pseudo-3D的结构，作者将3D kernel 结构替换成了Separating temporal convolution。经过试验发现两者的性能相差很少，但是计算量有很大的下降。

##### 第三个问题

最后作者基于对以上问题的探究，提出了gated spatiotemporal-separable 3D卷积 spatiotemporal-separable 3D卷积结构。

对于在空间坐标为$(w,h)$，以及时间帧$t$处的特征向量可以表征为：$X_{twh}\subset R^D$ 作者定义的gated操作为：

$$
{X^{'}} _{twh}= A \bigotimes X_{twh}
$$

其中$\bigotimes$表示为对应相乘，$A\subset R^D$ 是对应的权重(gate)。$A$的计算过程如下：

$$
A= \sigma(W \  pool_{st}(X))\ \ \ where\ \  W\subset R^{D\times D}
$$

#### 结束

新的state-of-the-art工作诞生了。