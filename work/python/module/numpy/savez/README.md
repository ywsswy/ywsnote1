压缩存多个numpy.ndarray数据
tang_array = np.array([[1,2,3],[4,5,6]])
np.savez('tang.npz',a=tang_array,b=tang_array2)
data = np.load('tang.npz')
data.keys()
['b', 'a']
data['a']
array([[1, 2, 3],
       [4, 5, 6]])
