numpy.vstack((a,b)) # 传入一个元组 
同
numpy.concatenate((a,b),axis = 0)
但是concatenate要求维度ndim相同
而vstack则智能
纵向拼接
