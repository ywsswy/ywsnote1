【超平面
平的，不是曲的
【SVM
以二维为例
两条平行的两个类划分边界（边界上的点为支撑向量）的切线，
中间距离为margin=2*d，根据在线的哪一测来分类，才使模型泛化能力强
d是中垂线
【Hard Margin SVM
线性可分的（有条线能分开数据）
min((1/2)*||w||^2) 
s.t. yi(wT*xi+b) >= 1
【Soft Margin SVM
L1正则
min((1/2)*||w||^2 + CΣζi) 
s.t. yi(wT*xi+b) >= 1-ζi ζi >= 0
L2正则
min((1/2)*||w||^2 + CΣ(ζi^2)) 
s.t. yi(wT*xi+b) >= 1-ζi ζi >= 0
