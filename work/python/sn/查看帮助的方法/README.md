import sys
import PyQt5
from PyQt5 import QtGui
out = sys.stdout
sys.stdout = open(r'D:\ynote.txt', 'w')
help(PyQt5.QtGui)
sys.stdout.close()
sys.stdout = out
