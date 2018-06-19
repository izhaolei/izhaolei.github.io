---
title: PyTorch 固定层 finetune
mathjax: true
type: categories
categories:  PyTorch4.0
---

### fix layer in PyTorch

在神经网络finetune中往往需要固定pretrained model中的某些层，然后再训练，这样可以避免数据不足带来的问题。

在mxnet等框架中，可以十分方便的固定某些层，但是在PyTorch中相对变得繁琐。这里提供一种相对折中的方法：以固定网络中的BatchNorm为例：

```python
def set_bn_fix(m):
    classname = m.__class__.__name__
    if classname.find('BatchNorm') != -1:
        for p in m.parameters(): p.requires_grad=False
model.apply(set_bn_fix)
```

在训练的时候使用$filter$函数将需要优化的参数筛选出来传入即可。

当然固定BatchNorm后应该吧BatchNorm设置为$eval$模式：

```
def set_bn_eval(m):
    classname = m.__class__.__name__
    if classname.find('BatchNorm') != -1:
        m.eval()
model.apply(set_bn_eval)
```

