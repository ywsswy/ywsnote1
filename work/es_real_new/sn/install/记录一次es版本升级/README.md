旧版本es：
/usr/share/elasticsearch/lib/elasticsearch-log4j-7.16.1.jar:2.11.1，（会被检测漏洞，ps腾讯云检测漏洞的方法是所有.jar后缀的文件都会被解压，然后看里面的组件版本是否有问题）
最好升级es到7.16.3以上


除了前面的elasticdump工具，这里实验一下es自带的快照功能

https://blog.csdn.net/Moonlet_/article/details/126509989
https://blog.csdn.net/hehe8881/article/details/121652483