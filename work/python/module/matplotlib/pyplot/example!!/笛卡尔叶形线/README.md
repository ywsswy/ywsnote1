import numpy as np
import matplotlib.pyplot as plt
ll = 0.0009
t = np.linspace(-10,10,999999)
x = 3*t/(1+t**3)
y = 3*t**2/(1+t**3)
plt.plot(x,y)
plt.xlim(-2,2)
plt.ylim(-2,2)
plt.axvline(0,color='red')
plt.axhline(0,color='red')
plt.show()
