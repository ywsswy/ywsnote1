【
Confusion Matrix
混淆矩阵
true false 预测对还是错
negative posive 预测无病（0类）还是有病（1类）
TN	FP
FN	TP
【
精准率 != 准确率(TP+TN)/ALL
precision = TP/(TP+FP)，预测有病中，预测对了的
适用股票
召回率
recall = TP/(TP+FN)，有病中，预测对了的
适用诊断
【F1 Score
精准率和召回率的调和平均值（调和平均值能兼顾二者，只有二者都很高的时候才高）
1/F1 = (1/2)*((1/precision)+(1/recall))
【
Precision-Recall的平衡
调整阈值
θx = threshhold
【
PR曲线
x轴precision y轴recall
曲线越往外侧模型越好
【
TPR = recall =  TP/(TP+FN)
有病中，预测对了的
FPR = FP/(TN+FP)
没病中，误诊了的
【
ROC曲线
x轴FPR y轴TPR
对有偏数据不太敏感
