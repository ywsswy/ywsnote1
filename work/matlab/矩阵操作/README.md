a=[1,2,-4;-2,2,1;-3,4,-2] # 注意逗号还是分号的区别可能会影响成为行向量或列向量
b=det(a)
b= a'%转置矩阵
b=inv(a)%逆矩阵
zeros(m,n)%全零矩阵
zeros(N)
b=zeros(size(a))
ones(m,n)%全一矩阵
eye(m,n)%单位矩阵（主对角线为1）
diag(X)%对角矩阵
%diag(X) == diag(X,m)(m==0);m为对角线头向右偏移的行数
triu（X,K)%上/下三角阵
%triu(X,0) = triu(X) 
rand(m,n)%随机矩阵元素（0.0~1.0之间）
inv(a)%求逆
format rat%以分数形式显示
A*B%矩阵乘法
A.*B%对应元素相乘

