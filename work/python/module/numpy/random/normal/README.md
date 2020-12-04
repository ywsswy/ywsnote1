高斯分布
np.random.normal()#默认0到1之间
import numpy
import numpy as np
mu, sigma = 0,0.1
np.random.normal(mu,sigma,10)#生成一个size为10，均值为0，方差为0.1的随机数组
