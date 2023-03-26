对感兴趣的大小次序排序
比如获取第三大的索引
import numpy
numpy.random.seed(0)
numpy.set_printoptions(precision = 4)
z = numpy.random.randint(20,50,4)
print(z)
b = numpy.argpartition(z,[-3])
print(b[-3])
比如获取最大的和最小的值
import numpy
numpy.random.seed(0)
numpy.set_printoptions(precision = 4)
z = numpy.random.randint(20,50,4)
print(z)
b = numpy.argpartition(z,[0,-1])
print(r'''max is {}'''.format(z[b[-1]]))
print(r'''min is {}'''.format(z[b[0]]))
#类似快排中的一轮循环
numpy.argpartition(z,index)
index能获取第几小的数据，并且，比其小的都在左边，比其大的都在其右边
比如获取top3大的值
import numpy
numpy.random.seed(0)
z = numpy.random.randint(10,20,10)
print(z)
c = numpy.argpartition(z,-2)
print(c)
print(c[:-2-1:-1])
print(z[c[:-2-1:-1]])
