#!/usr/bin/env python3


class YGlobal(object):
    import pynput
    start_position = (469, 241)
    times = 13
    mouse_controller_ = pynput.mouse.Controller()
    dd_pixel = 44
    dd_second = 0.4

def ButtonLeft(press_flag):
    import pynput
    if press_flag:
        YGlobal.mouse_controller_.press(pynput.mouse.Button.left)
    else:
        YGlobal.mouse_controller_.release(pynput.mouse.Button.left)
    return None
    

def Main():
    import pynput
    import time
    YGlobal.mouse_controller_.position = (YGlobal.start_position)
    for i in range(0, YGlobal.times):
        ButtonLeft(True)
        ButtonLeft(False)
        YGlobal.mouse_controller_.move(0,YGlobal.dd_pixel)
        time.sleep(YGlobal.dd_second)

def Main2():
    import time
    for i in range(0,100):
        position=format(YGlobal.mouse_controller_.position)
        print(position)
        time.sleep(0.1)
Main()