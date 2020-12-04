# 插入一点，网络连接都是使用电脑自带的PPPoE拨号上网
# ipconfig里已断开的媒体连接不用看，以图形界面的网络连接/网络适配器为准
# cmd.exe(administrator)
netsh wlan show drivers 
//显示系统上无线 LAN 驱动程序的属性
其中，“支持的承载网络(hostednetwork/wifi)  : 是”表示可以共享，否则不可以共享
保持可以共享的无线网卡处于启动状态
netsh wlan set hostednetwork mode=allow ssid=SingleDog key=xiaoshanshan
//设置承载网络的属性（此时网络适配器中会多出一个hosted类型的网络，但显示未连接，手机也搜不到）
netsh wlan start hostednetwork
//启动承载网络（此时网络适配器中的hosted类型的网络显示有了名字，手机搜得到，但是连上也没外网）
在PPPoE属性，共享给上述hosted网络
