import cv2
import numpy as np
def nothing(x):
    pass
img=np.zeros((300,512,3),np.uint8)
cv2.namedWindow('image')
cv2.createTrackbar('R','image',0,255,nothing)
while(1):
    cv2.imshow('image',img)
    k=cv2.waitKey(1)&0xFF
    if k==27:
        break
    
    r=cv2.getTrackbarPos('R','image')
    img[:]=[255,255,r]
   
cv2.destroyAllWindows()
#//////////////////////////////////////////////////////////////////////
https://www.cnblogs.com/jerrybaby/p/5849625.html
#//////////////////////////////////////////////////////////////////////
