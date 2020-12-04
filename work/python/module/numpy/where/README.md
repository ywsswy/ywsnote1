2）用条件获取 下标索引或者（bool掩码），然后用下标索引或者（bool掩码）提取（副本）
a = numpy.array([1,2,3])
b = a > 1 #array([False,  True,  True], dtype=bool)
c = numpy.where(b) #(array([1, 2], dtype=int64),)
a[b]
或者
a[c]
