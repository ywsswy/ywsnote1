用多机器学习方法，然后进行投票
保证差异性
每个子模型都只看一部分数据，模型多的时候就不需要每个子模型有很高准确率
【
不放回取样
pasting
【放回取样，更常用
bagging
统计学中bootstrap
OOB Out of Bag
有37%的样本没有取到，直接把这个当作测试数据集
可以并行计算多个模型
随机取样
base estimator 都是决策树，组成在一起就是【随机森林】
更随机的extra-trees
ada boosting不断调整数据权值，以增强模型
gradient boosting 不断对错误进行训练
stacking

