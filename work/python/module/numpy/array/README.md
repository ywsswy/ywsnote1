创建数组
把 list（只能传一个list，但是可以是多维的）转为 numpy.ndarray
array(...)
    array(object, dtype=None, copy=True, order='K', subok=False, ndmin=0)
    
    Create an array.
    
    Parameters
    ----------
    object : array_like
        An array, any object exposing the array interface, an object whose
        __array__ method returns an array, or any (nested) sequence.
import numpy
a = numpy.array([1,2,3,4]) #一维，相当于1行4列，a.shape却显示的是(4,)
c = numpy.array([[1],[2],[3],[4]]) #注意有两个方括号#此时才是二维，4行1列，b.shape是(4,1)
b = numpy.array([0,0,1],dtype=bool)
#array([False,False,True])

