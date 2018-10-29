### norm methods
在深度学习中常见的norm方法如下： 

![](https://github.com/izhaolei/images/blob/master/normmethod.png?raw=true)

目前来讲，上述四种方法均可以用以下数学公式进行概述：

1.$$\quad \hat {x_i}=\frac{x_i-\mu_i}{\sigma} $$

2.$\quad y_i=\alpha * \hat{x_i} + \beta$

其中$\mu_i $与$\sigma$表示均值与方差，不同的方法只是在计算均值与方差的方式不同(在不同的维度上计算)， 同时为了使得norm不改变网络的表达，同时使用$\alpha$、$\beta​$ 进行仿射变换。 


#### batch norm

batch norm主要是在channel这个维度上归一化，换而言之在$[N,W,H]$这三个维度上计算 $\mu_i $与$\sigma$。

#### Layer Norm

Layer norm 主要是在 batch这个维度上归一化，即在$[C,W,H]$这三个维度上计算 $\mu_i $与$\sigma$。

#### Instance Norm

IN主要是在$[W,H]$上计算 $\mu_i $与$\sigma$。

#### Group Norm

GN主要用于解决BN在小batchsize的时候计算出来的 $\mu_i $与$\sigma$偏差太大的问题，计算的时候对于一层特征$[N,C,W,H]$首先将这层特征reshape成$[N,G,C//G,W,H]$，然后在$[C//G,W,H]$上计算 $\mu_i $与$\sigma$。