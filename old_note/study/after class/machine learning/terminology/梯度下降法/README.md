不是机器学习算法
是一种基于搜索的最优化算法（因为有的参数并不像线性回归那样参数可以用数学公式直接表示，而是需要不断搜索）
最小化一个损失函数
【
梯度上升法
最大化一个效用函数
损失函数J
-ηdJ/dθ
超参数 η学习率
【批量梯度下降法Batch Gradient Descent
每一次下降都要计算所有样本
【随机梯度下降法Stochastic Gradient Descent
每次算一个，轮流算
学习率η 要随着下降次数逐渐递减
（模拟退火的思想）
η = a/(i_iters+b) #i_iters为循环次数
可跳出局部最优解，快速，
【小批量 Mini-Batch
综合上两种
