"""Softmax."""
scores = [3.0, 1.0, 0.2]#假设输出结果是这个
import numpy as np
def softmax(x):
    """Compute softmax values for each sets of scores in x."""
    pass  # TODO: Compute and return softmax(x)
    return np.exp(x)/np.sum(np.exp(x),axis=0)
print(softmax(scores))#【3.0，1.0，0.2】对应的概率结果
#画出【x，1.0，0.2】(-2<x<6)的softmax计算结果曲线
# Plot softmax curves
import matplotlib.pyplot as plt
x = np.arange(-2.0, 6.0, 0.1)
#创建数组
scores = np.vstack([x, np.ones_like(x), 0.2 * np.ones_like(x)])
#构造跟x数组同大小的恒1恒0.2数组
plt.plot(x, softmax(scores).T, linewidth=2)
#计算softmax并转置成宽度为3的一堆数组，并绘制
plt.show()

