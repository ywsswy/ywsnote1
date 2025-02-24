深度学习框架PyTorch 既可以看做加入了GPU 支持的numpy，同时也可以看成一个拥有自动求导功能的强大的深度神经网络



- Tensor(张量)：PyTorch中的基本数据结构，类似于numpy数组。PyTorch的大部分计算和数据操作都是基于张量进行的

```
import torch
import numpy as np

x = np.arange(12).reshape(2,6)
x = torch.Tensor(x)
print(x)
"""
tensor([[ 0.,  1.,  2.,  3.,  4.,  5.],
        [ 6.,  7.,  8.,  9., 10., 11.]])
"""

```
