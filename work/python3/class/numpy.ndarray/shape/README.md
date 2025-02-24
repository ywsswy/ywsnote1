[[ 1 11 21]
 [ 2 12 22]]
(2, 3)
a = numpy.array([[1,11,21],[2,12,22]])
print(a)
print(a.shape)#获取每一维的长度
可以修改形状
a.shape = 3,2 #改成3行，2列
a.reshape(3,2)

