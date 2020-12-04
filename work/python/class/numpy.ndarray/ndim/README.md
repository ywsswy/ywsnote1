维度
tang_array = np.arange(10)
tang_array.shape # (10,)
tang_array.ndim # 1
tang_array = tang_array[np.newaxis,:]#一个新维度，把所有（已有的一行）放进去，变成了二维
tang_array.shape # (1,10)
tang_array.ndim # 2
tang_array = tang_array[:,np.newaxis]#所有的（10列）分别放到一个新维度中，变成了二维
tang_array.shape # (10,1)
tang_array.ndim # 2
tang_array = tang_array[:,np.newaxis,np.newaxis]
tang_array.shape #(10, 1, 1, 1)

