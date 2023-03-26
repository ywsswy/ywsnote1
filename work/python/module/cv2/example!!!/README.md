cv2.namedWindow("f1",cv2.WINDOW_NORMAL) #可改变大小的窗口
ar = cv2.imread('D:\\test.jpg', 0) #<class 'numpy.ndarray'> # 0代表灰度图，默认RGB
cv2.imshow("f1", ar)
cv2.waitKey (0)
cv2.destroyAllWindows()
