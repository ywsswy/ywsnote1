去掉小数部分后的浮点数
import numpy as np
z = np.random.uniform(0,10,10)
b = np.floor(z)
a = b[2]
a
