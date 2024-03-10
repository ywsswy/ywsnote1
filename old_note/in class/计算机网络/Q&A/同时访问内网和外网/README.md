route add 202.202.32.0 mask 255.255.240.0 172.33.0.1
route delete 202.202.32.0
【
.bat
cls
@ECHO OFF
CLS
color 0a
echo 使用说明如下：
echo 1、以管理员身份运行此程序
echo 2、程序会需要你输入默认网关，下面你将看到的“默认网关. . .”或者“Default Gateway. . .”后面以172或202开头的数字就是了。
echo 3、程序结束后再登陆外网
pause
echo =======================================================================
ipconfig 
echo =======================================================================                
echo 请输入上面显示的你的校园网网关IP
set /p gateway=
echo 你输入的校园网网关IP是 %gateway%
pause
route delete 202.202.32.0 mask 255.255.240.0 
route delete 172.0.0.0 mask 255.192.0.0 
route delete 172.16.0.0 mask 255.240.0.0 
route delete 172.16.0.0 mask 255.224.0.0 
route delete 172.32.0.0 mask 255.254.0.0 
route delete 211.83.208.0 mask 255.255.240.0 
route delete 222.177.140.0 mask 255.255.255.128 
route delete 219.153.62.64 mask 255.255.255.192 
route delete 10.10.10.0 mask 255.255.255.0 
route delete 192.168.0.0 mask 255.255.0.0
route add -p 202.202.32.0 mask 255.255.240.0 %gateway%
route add -p 172.16.0.0 mask 255.224.0.0 %gateway%
route add -p 172.32.0.0 mask 255.254.0.0 %gateway%
route add -p 211.83.208.0 mask 255.255.240.0 %gateway%
route add -p 222.177.140.0 mask 255.255.255.128 %gateway%
route add -p 219.153.62.64 mask 255.255.255.192 %gateway%
route add -p 10.10.10.0 mask 255.255.255.0 %gateway%
route add -p 192.168.0.0 mask 255.255.0.0 %gateway%
echo 路由添加完毕，现在你应该可以上校园网了
pause
