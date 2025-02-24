避免劣化代码（一写出来就不合理）
避免用大小写来区分
避免易混淆：已存在于上下文中、类似内建名（list、element）、o与l、的名称
不要害怕过长的变量名

更好的
函数 动词_名词 find_num
变量 名词名词（除了第一个外首字母大写）

业界公认的Pythonic代码：Flask gevent requests

PEP8风格检查程序
pip install -U pep8
pep8 --first optparse.py #自己的py程序
pep8 --show-source --show-pip8 my.py
