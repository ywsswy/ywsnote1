sudo nohup python -u 

nohup <cmd> >nohup.log 2>&1 & #（后台执行cmd）//cmd是python的话要python -u; 2(错误信息)重定向到1（标准输出）

jobs可以查看后台[running/stoped]的程序，

fg [num] 可以把后台程序放到【前台】运行
bg [num] 可以把后台程序放到【后台】运行
ctrl+z可以把前台程序停止并放到后台

screen+上述操作即可搞事情