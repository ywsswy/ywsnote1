import numpy
import mpl_toolkits.mplot3d #三维中需要新的轴
import matplotlib.pyplot
x = numpy.linspace(-50,50,100)
y = numpy.linspace(-50,50,100)
x,y = numpy.meshgrid(x,y)
z = x+y #numpy.fabs(x) + numpy.fabs(y)
ax = mpl_toolkits.mplot3d.Axes3D(matplotlib.pyplot.figure());'''a new board''';ax.plot_surface(x,y,z, rstride=1, cstride=1, cmap='rainbow')
ax.view_init();matplotlib.pyplot.title('default:30,-60')
matplotlib.pyplot.xlabel('x');matplotlib.pyplot.ylabel('y');ax.set_zlabel('z');matplotlib.pyplot.show()

