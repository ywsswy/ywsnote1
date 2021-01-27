在没有sudo权限的电脑上怎么跑Python

```
wget https://bootstrap.pypa.io/get-pip.py
python3 get-pip.py --user

pip3 install flask --user
或者python3 -m pip install --user flask <-i 源>
三个国内源
阿里云：https://mirrors.aliyun.com/pypi/simple/
豆瓣：https://pypi.douban.com/simple/
清华：https://pypi.tuna.tsinghua.edu.cn/simple/
腾讯：https://mirrors.cloud.tencent.com/pypi/simple
...
```


还有一种直接源码安装库的方法，直接把文件夹放入安装目录，如（.local/lib/python3.6/site-packages）。
文件夹格式类似
```
about.py
include
__init__.pxd
__init__.py
mrmr.cpython-37m-x86_64-linux-gnu.so
mrmr.pxd
mrmr.py
mrmr.pyx
__pycache__
tests
```

python -m SimpleHTTPServer <port> python牛逼，这种就是提供http服务端，/是列出当前目录的文件