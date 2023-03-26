import numpy as np
from math import sqrt
from collections import Counter
class KNNClassifier:#类名，不需要和文件名相同
#构造函数__init__(self,...):
    def __init__(self, k):
        """初始化kNN分类器"""
        assert k >= 1, "k must be valid"
        self.k = k #成员变量self.局部变量正常
        self._X_train = None #私有变量加下划线
        self._y_train = None
#其他函数正常fun(self,...):
    def fit(self, X_train, y_train):
        """根据训练数据集X_train和y_train训练kNN分类器"""
        assert X_train.shape[0] == y_train.shape[0], \
            "the size of X_train must be equal to the size of y_train"
        return self #返回自身对象
#私有函数加下划线
    def _predict(self, x):
        return None
#对象的自身打印值
    def __repr__(self):
        return "KNN(k=%d)" % self.k
#对象的自身print值
    def __str__(self):
	return "KNN"
