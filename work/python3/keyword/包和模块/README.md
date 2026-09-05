【模块】就是一个py文件

使用a.py的方法就是import a，然后调用a.py里面def的函数


【（导入）包】就是一个文件夹（例如b文件夹里放了c.py）
包的目录中通常包含一个特殊的文件 __init__.py，它可以是空的
使用b包的方法就是from b import c


包分两种：
- 【分发包】pip3 命令写的名字
- 【（导入）包】安装后包的目录名
pip3 install pyOpenSSL后，实际安装到site-packages 目录里有两个目录：
一个是pyOpenSSL-a.b.c-py3.x.egg-info，这个是元数据，元数据目录里有个PKG-INFO（能看分发包名称是pyOpenSSL），还有个top_level.txt（能看导入包名称是OpenSSL）；
另一个是OpenSSL，这个就是导入包了；代码里使用时要写from OpenSSL，而不是from pyOpenSSL；

自己打包：
python3 setup.py sdist  # 会打包出dist/<包>-<版本>.tar.gz，分发出去后，用户 pip3 install <包>-<版本>.tar.gz 时会在本地跑一次 setup.py；
python3 setup.py bdist_wheel # 会打包出dist/<包>-<版本>-py3-none-any.whl，分发出去后，用户 pip3 install 直接解压即可；


pip命令 & 安装
- python3 -m pip show <分发包名>  # 查看某个分发包的安装目录，安装的原理相当于把源码复制一份到site-packages 目录里
- 同一个包安装多次（不同版本）只会有一个存在（最后安装的那个版本覆盖其他版本）
- python3 -m pip install --upgrade pip  # 升级pip版本
- pip默认安装包时是从源上下载安装包，也可以：
（1）手动从软件源下载whl文件，然后 python3 -m pip install <whl_file>
（2）直接把源码文件夹（正确的格式里面会有一个__init__.py）放入安装目录里（如 /usr/local/lib/python3.6/site-packages）
- 不用pip系统安装的使用方法（例如开发者自己自己的开发包，处理调试阶段）：
（1）不安装，只是当前目录有，pip3 show是看不到的；
（2）editable 安装（相当于本地软连接到site-packages 目录里，一般叫<包名>.egg-link），场景是本地开发实时调试时使用；感觉不好用

- 在没有sudo权限的电脑上怎么跑Python：

```
wget https://bootstrap.pypa.io/get-pip.py
python3 get-pip.py --user

python3 -m pip install --user flask <-i 源>
三个国内源
阿里云：https://mirrors.aliyun.com/pypi/simple/
豆瓣：https://pypi.douban.com/simple/
清华：https://pypi.tuna.tsinghua.edu.cn/simple/
腾讯：https://mirrors.cloud.tencent.com/pypi/simple
https://mirrors.tencent.com/repository/pypi/tencent_pypi/simple
https://mirrors.tencent.com/pypi/simple
...
```