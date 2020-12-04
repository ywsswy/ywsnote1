import sys
import os
sys.path.append(os.path.abspath('.')+r'''\ymodules''')
print(sys.path)
del os
del sys
//////////////////////////////////////////////////////////////////////
#导入某文件夹下的模块
import sys
sys.path.append('./mymodules')#先把其目录设到环境变量中
import ym1
