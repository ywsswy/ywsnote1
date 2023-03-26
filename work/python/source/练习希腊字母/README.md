import numpy
in1 = numpy.arange(913,938)
in2 = in1[in1!=930].copy()
in1 = numpy.arange(945,970)
in2 = numpy.hstack((in1,in2))
numpy.random.shuffle(in2)
for i in in2:
    print(chr(i))
    print(i)

