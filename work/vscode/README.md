Host 随便写
  HostName 目标ip
  Port 目标端口
  User 目标用户
  IdentityFile 本机私钥地址，目标机器应该存了公钥


图形化的设置，右上角一般有看json格式的设置




两处设置
【C/C++ edit configuration】这只是本项目.vscode目录（c_cpp_properties.json）可以设置代码include路径，c++标准版本
【preferences】另一处是（settings.json）这个可以设置user（存到本地，统一都用这个就好）或者远端（鼠标悬停可以看到文件地址）

windows系统要开启OpenSSH Authentication Agent服务
windows系统使用vim开启ctrl+c复制的时候必须要normal模式进行，不能insert模式下；(terminal里面是ctrl+c复制，ctrl+v粘贴，cmd等其他终端里面是鼠标双击复制，ctrl+v粘贴)


# 可以远程gdb
https://blog.csdn.net/qq_40181728/article/details/108079893
光标停在项目根目录的一个文件，然后run->add configurations
生成的launch.json中修改如下信息
"program": "<path>/<to>/<binary>",
            "args": [],
            "stopAtEntry": true,