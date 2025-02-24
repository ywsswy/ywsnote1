边界填充
tang_array = np.ones((5,5))
tang_array = np.pad(tang_array,pad_width = 1,mode = 'constant',constant_values = 0)
tang_array
array([[ 0.,  0.,  0.,  0.,  0.,  0.,  0.],
       [ 0.,  1.,  1.,  1.,  1.,  1.,  0.],
       [ 0.,  1.,  1.,  1.,  1.,  1.,  0.],
       [ 0.,  1.,  1.,  1.,  1.,  1.,  0.],
       [ 0.,  1.,  1.,  1.,  1.,  1.,  0.],
       [ 0.,  1.,  1.,  1.,  1.,  1.,  0.],
       [ 0.,  0.,  0.,  0.,  0.,  0.,  0.]])
#边界填充宽为1的值为0的 mode为constant的数 
mode: str or function
        One of the following string values or a user supplied function.
    
        'constant'
            Pads with a constant value.
