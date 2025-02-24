flags
    Information about the memory layout of the array.
    
    Attributes
    ----------
    C_CONTIGUOUS (C)
        The data is in a single, C-style contiguous segment.
    F_CONTIGUOUS (F)
        The data is in a single, Fortran-style contiguous segment.
    OWNDATA (O)
        The array owns the memory it uses or borrows it from another object.
    WRITEABLE (W)
构造一个数组，让它不能被改变
z = np.zeros(5)
z.flags.writeable = False
z[0] = 1
