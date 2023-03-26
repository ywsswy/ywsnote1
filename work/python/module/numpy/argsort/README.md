获取排序后的名次（可以用这个名次作为索引，去输出排序后的数据），不会改变原数据
a1 = numpy.array([11,10,12])
a2 = numpy.argsort(a1)
a2 #array([1, 0, 2], dtype=int64)
