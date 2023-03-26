限制参数大小
【正则化
Regularization
【
岭回归：目标函数中加入α*(1/2)Σ(θi)^2
Ridge-Regression
J(θ) = MSE(y,y^;θ)+α*(1/2)Σ(θi)^2
L2正则项
【
LASSO-Regression
J(θ) = MSE(y,y^;θ)+α*Σ|θi|
弯曲程度低，趋向于使得一部分 θ变为0，所以具有特征选择的作用
L1正则项
【
min{number-of-non-zero-θ}
限制θ的个数
L0正则项
【弹性网 Elastic Net
结合岭回归和LASSO回归
r* α*Σ|θi|   + ((1-r)/2)*α*Σ(θi)^2
【逻辑回归中的正则化
C1 * J(θ) + L1
C2 * J(θ) + L2
