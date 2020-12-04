# 开箱步骤
- 安装，之后【任何】操作都是在sudo su之后的操作！！！！！（这样有操作windows文件的权限/mnt/d/）
- 开启ssh服务，用xshell登陆（操作方法参考ssh连接）
- 基本工具trash-put等安装 sudo python3 setup.py install
# 其他
- wsl中操作U盘
WSL mkdir /mnt/g; sudo su; mount -t drvfs G:\\ /mnt/g; #git 不支持fat32文件系统且要在sudo su下
- wsl中重定向输出到文件中，tail -f 可能无法实时看到的（/mnt/*目录下）
- 重启 net stop LxssManager

中文支持问题，不支持的话可能显示有问题，python编码解码也有问题
确保locale的结果是en_US.utf8，否则自行解决。。。。。（sudo dpkg-reconfigure locales）
