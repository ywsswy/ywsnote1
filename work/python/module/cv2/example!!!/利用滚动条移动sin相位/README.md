## 自己画坐标轴了，所以用到了plt
import cv2
import numpy as np
import io
import PIL
import matplotlib.pyplot as plt
def fig2ndimg(fig):
    buffer_ = io.BytesIO()
    fig.savefig(buffer_, format = "png")
    buffer_.seek(0)
    image = PIL.Image.open(buffer_)
    img = np.asarray(image)
    buffer_.close()
    return img
def nothing(x):
    pass
fig1 = plt.figure(1)
cv2.namedWindow("window1",cv2.WINDOW_NORMAL) 
cv2.createTrackbar('theta','window1',0,180,nothing)
while True:
    theta=cv2.getTrackbarPos('theta','window1')
    x = np.linspace(0,np.pi*2,99)
    y = np.sin(x+theta*np.pi/180)
    ax1 = fig1.add_subplot(1,1,1)
    ax1.cla()
    ax1.set_title('test1')
    ax1.plot(x,y)
    i1 = fig2ndimg(fig1)
    cv2.imshow("window1", i1)
    k=cv2.waitKey(1)&0xFF
    if k == 27 or k == 13:
        break
cv2.destroyAllWindows()
