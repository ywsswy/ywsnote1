import cv2
import numpy as np
class YGlobalData(object):
    YTITLE_X = 100
    YTITLE_Y = 20
    YTITLE_W = 240
    YTITLE_H = 80
    YIMG_X = 50
    YIMG_Y = 120
    YIMG_W = 340
    YIMG_H = 300
    YBUTTON1_X = 20
    YBUTTON2_X = 240
    YBUTTON_Y = 450
    YBUTTON_W = 150
    YBUTTON_H = 70
    YWIN_H = 600
    YWIN_W = 440
    
# 设置回调参数，event是事件类型，x,y是鼠标坐标，param是setMouseCallback第三个参数的“引用”
def print_test(event, x, y, flags, param):
    if event == cv2.EVENT_FLAG_LBUTTON:
        if x > YGlobalData.YBUTTON1_X and x < YGlobalData.YBUTTON1_X + YGlobalData.YBUTTON_W\
        and y > YGlobalData.YBUTTON_Y and y < YGlobalData.YBUTTON_Y + YGlobalData.YBUTTON_H:
            param[0][YGlobalData.YIMG_Y : YGlobalData.YIMG_Y + YGlobalData.YIMG_H\
                    ,YGlobalData.YIMG_X : YGlobalData.YIMG_X + YGlobalData.YIMG_W] = param[1].copy()
        if x > YGlobalData.YBUTTON2_X and x < YGlobalData.YBUTTON2_X + YGlobalData.YBUTTON_W\
        and y > YGlobalData.YBUTTON_Y and y < YGlobalData.YBUTTON_Y + YGlobalData.YBUTTON_H:
            param[0][YGlobalData.YIMG_Y : YGlobalData.YIMG_Y + YGlobalData.YIMG_H\
                    ,YGlobalData.YIMG_X : YGlobalData.YIMG_X + YGlobalData.YIMG_W] = param[2].copy()
    
def main():
    img_win = np.zeros((YGlobalData.YWIN_H,YGlobalData.YWIN_W,3),np.uint8)
    cv2.namedWindow("windows_test",cv2.WINDOW_NORMAL)
    img1 = cv2.imread('test1.jpg')
    img2 = cv2.imread('test2.jpg')
    button1 = cv2.imread('button1.jpg')
    button2 = cv2.imread('button2.jpg')
    title = cv2.imread('title.jpg')
    img1 = cv2.resize(img1, (YGlobalData.YIMG_W,YGlobalData.YIMG_H))
    img2 = cv2.resize(img2, (YGlobalData.YIMG_W,YGlobalData.YIMG_H))
    button1 = cv2.resize(button1, (YGlobalData.YBUTTON_W,YGlobalData.YBUTTON_H))
    button2 = cv2.resize(button2, (YGlobalData.YBUTTON_W,YGlobalData.YBUTTON_H))
    title = cv2.resize(title, (YGlobalData.YTITLE_W,YGlobalData.YTITLE_H))
    img_win[YGlobalData.YTITLE_Y : YGlobalData.YTITLE_Y + YGlobalData.YTITLE_H\
            ,YGlobalData.YTITLE_X : YGlobalData.YTITLE_X + YGlobalData.YTITLE_W] = title.copy()
    img_win[YGlobalData.YBUTTON_Y : YGlobalData.YBUTTON_Y + YGlobalData.YBUTTON_H\
            ,YGlobalData.YBUTTON1_X : YGlobalData.YBUTTON1_X + YGlobalData.YBUTTON_W] = button1.copy()
    img_win[YGlobalData.YBUTTON_Y : YGlobalData.YBUTTON_Y + YGlobalData.YBUTTON_H\
            ,YGlobalData.YBUTTON2_X : YGlobalData.YBUTTON2_X + YGlobalData.YBUTTON_W] = button2.copy()
    img_win[YGlobalData.YIMG_Y : YGlobalData.YIMG_Y + YGlobalData.YIMG_H\
            ,YGlobalData.YIMG_X : YGlobalData.YIMG_X + YGlobalData.YIMG_W] = img1.copy()
    cv2.setMouseCallback('windows_test', print_test, [img_win,img1,img2])
    while True:
        cv2.imshow("windows_test", img_win) 
        if cv2.waitKey(20) & 0xFF == 27: # Esc 键退出
            break
    cv2.destroyAllWindows()
main()
