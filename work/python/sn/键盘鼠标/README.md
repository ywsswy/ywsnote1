pyautogui # 截屏，兼容系统，控制

# 听键盘：
pynput # 监听(不够底层，有些软件抓不到）
system_hotkey #独占系统热键（依赖win）

# 听鼠标：
PyQt5，全局的鼠标坐标只能用定时器做

# 移动鼠标：
pyautogui X
PyQt5 X
pynput X
pyhook X(3.8不支持)
pyhook3 X

# 没试过
PyUserInput



from pynput.keyboard import Key, Controller

keyboard = Controller()

#Press and release space
#keyboard.press(Key.space)
#keyboard.release(Key.space)
# or 
'''
with keyboard .pressed(Key.ctrl):
    keyboard.press('r')
    keyboard.release('r')
'''
#type 'hello world '  using the shortcut type  method
#keyboard.type('hello world')
keyboard.press(Key.f10)
keyboard.release(Key.f10)

keyboard.press(Key.space)
keyboard.release(Key.space)

#import webbrowser
#webbrowser.open_new_tab('https://rd5.zhaopin.com/custom/register?za=2&ps=1')

