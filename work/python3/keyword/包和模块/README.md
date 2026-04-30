模块就是一个py文件

使用a.py的方法就是import a，然后调用a.py里面def的函数


包就是一个文件夹（例如b文件夹里放了c.py）
包的目录中通常包含一个特殊的文件 __init__.py，它可以是空的
使用b包的方法就是from b import c



使用的方式有几种：
1）系统安装，pip3 install（相当于把源码复制一份到 Python 的 site-packages 目录里），pip3 show <包名>能看到安装目录；
2）不安装，只是当前目录有，pip3 show <包名>是看不到的；
3）editable 安装（相当于本地软连接到site-packages 目录里，一般叫<包名>.egg-link），场景是本地开发实时调试时使用；感觉不好用


python3 setup.py sdist  # 会打包出dist/<包>-<版本>.tar.gz，分发出去后，用户 pip3 install <包>-<版本>.tar.gz 时会在本地跑一次 setup.py；
python3 setup.py bdist_wheel # 会打包出dist/<包>-<版本>-py3-none-any.whl，分发出去后，用户 pip3 install 直接解压即可；
