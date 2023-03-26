a = numpy.arange(0,100,10)
numpy.random.shuffle(a)
numpy.partition(a,3)
#排序，使得比第四小的数还小的数都在前面，比第四小的数还大的数都在后面
功能上类似，第四小的数是一个快排的中轴点
