逐层判断
根据某一维度上的某个阈值依次划分
【信息熵
H = -pi*log(pi)
计算稍慢
【基尼系数
G = 1-Σ((pi)^2)
越低确定性最高
sklearn默认是此
【CART
根据某一维度上的某个阈值依次划分
sklearn是此
【复杂度
n维度 m数据
预测时：O(logm)层
训练：O(n*m*logm)
剪枝：降低复杂度，解决过拟合
【解决回归问题
先分类，在类中算平均值来作为预测值
【局限性
横平竖直
对个别数据点敏感
