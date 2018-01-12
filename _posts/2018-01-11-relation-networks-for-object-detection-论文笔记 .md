---
title: relation networks for object detection 论文笔记
mathjax: true
type: categories
categories: paper-note
---

​     本文是2017年底微软亚洲研究院的工作，是一个目标检测的框架，号称**真正**做到了目标检测的端到端模型。本文提出的relation networks 借鉴了自然语言处理任务中常用的attention机制。建立了不同目标之间的相关性量化model，极大提高了检测的效率与性能。

#### 基本框架

​     目前主流的检测框架，主要分为两类：以R-CNN为代表的two-stage模型，和以SSD为代表的one-stage模型。即使像SSD这样的single shot的框架也需要NMS后处理步骤，本文提出的relation networks 避免了后处理的问题真正做到了端到端，如下图。

![](https://github.com/lxg2015/notes/raw/c410ec3323e5d99b3d58d4a5efe7bdf48cdd3e20/image/essay/relationmodule.jpg?raw=true)

​     在目标检测任务中，上下文信息，目标间关系可以改善算法的性能已经成为了一种共识，但是近年来随着深度学习方法的流行，这种共识的应用便很少出现在基于深度学习的检测框架之中，主要原因是目标物体之间的关系很难建模，比如目标位置、大小、类别、数目等。

​     本文主要受到自然语言处理任务中attention机制[[1]](https://arxiv.org/abs/1706.03762)的启发，本文将attention机制加以拓展，使之适用于目标检测中目标物体之间的关系衡量。

​     在自然语言处理的任务中，attention机制可以表征为：

$$
V^{out}=softmax(\frac{QK^t}{\sqrt{d_k}})V \ \ \ \ \ \ \ （1）
$$

​     文中作者将上式中的attention机制进行拓展，作者认为对于一个待检测的目标物体，包含两组特征：几何特征$f_G$ 、表征特征$f_A$。对于一组包含$N$个输入物体$\{({f_A^n},{f_G^n})\}_{n=1}^N$的relation network，网络输出的第n个relation feature 表征为：

$$
f_R(n)=\sum_m w^{mn} \cdot(W_V\cdot f_A^m)\ \ \ \ \ \ \ （2）
$$

​     在传统的attention机制中，式（2）中$(W_V\cdot f_A^m)$代表式（1）中的$V$。这里$w^{mn}$的计算过程便是论文的创新之处。

$$
w^{mn}=\frac{w_G^{mn}\cdot exp(w_A^{mn})}{ \sum_k w_G^{kn}\cdot exp(w_A^{kn})}
$$

​     相比于传统attention直接的softmax函数得到加权系数，这里的综合考虑了几何特征与表征特征两方面的影响，得到最终的加权系数$w^{mn}$。$w_G^{mn}$与$w_A^{mn}$的计算过程分别如下：

$$
w_A^{mn}=\frac{dot(W_kf_A^m,W_qf_A^n)}{\sqrt{d_k}}
$$

$$
w_G^{mn}=max({0,W_G\cdot {\varepsilon}_G(f_G^m,f_G^n)})
$$

​     最终的relation network结构如下图所示，是一个残差结构：

![](https://github.com/izhaolei/images/blob/master/relation.JPG?raw=true)

 

最终的检测模型分为两部分，一部分叫做enhanced 2fc head，另一部分叫做duplicate removal network。

![](https://github.com/izhaolei/images/blob/master/relation_2.JPG?raw=true)



#### reference

[1] Vaswani A, Shazeer N, Parmar N, et al. Attention Is All You Need[J]. 2017.