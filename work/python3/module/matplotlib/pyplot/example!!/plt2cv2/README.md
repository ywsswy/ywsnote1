import io
import numpy as np
import PIL
import matplotlib.pyplot as plt
import cv2
def fig2ndimg(fig):
    buffer_ = io.BytesIO()
    fig.savefig(buffer_, format = "png")
    buffer_.seek(0)
    image = PIL.Image.open(buffer_)
    img = np.asarray(image)
    buffer_.close()
    return img
img = cv2.imread('D:\\test.bmp', 0)
fig = plt.figure()
tlim = 9
t = np.linspace(-tlim,tlim,99999)
x = np.fabs(t)**(2/3)
y = t
plt.plot(x,y)
ar = fig2ndimg(fig)
cv2.namedWindow("my") 
cv2.imshow("my", ar) 
cv2.waitKey (0)
cv2.destroyAllWindows()
