import pynput


def OnPress(key):
# key有两种类型，要么可打印字符，用KeyCode表示；否则用enum表示
# <class 'pynput.keyboard._win32.KeyCode'>
# <enum 'Key'>
	print('{0} press'.format(key))

def OnRelease(key):
	print('{0} release'.format(key))
	# 不可打印的判断方法
	if key == pynput.keyboard.Key.scroll_lock:
		return False
	# 可打印的判断方法 if key == pynput.keyboard.KeyCode.from_char('a'):

with pynput.keyboard.Listener(on_press = OnPress,on_release = OnRelease) as listener:
	listener.join()
		
# keyboard1 = pynput.keyboard.Controller()
# keyboard1.press('A')
# keyboard1.release('A')
# https://pythonhosted.org/pynput/keyboard.html#monitoring-the-keyboard
