【交换机的端口工作模式】一般可以分为三种：Access，Multi，Trunk。
trunk模式的端口用于交换机与交换机，交换机与路由器，大多用于级联网络设备。
Access多用于接入层也叫接入模式。主要是将端口静态接入。 
cisco网络中，交换机在局域网中最终稳定状态的接口类型主要有四种：access/ trunk/ multi/ dot1q-tunnel。  
 
1、access: 主要用来接入终端设备，如PC机、服务器、打印服务器等。   
2、trunk: 主要用在连接其它交换机，以便在线路上承载多个vlan。   
3、multi: 在一个线路中承载多个vlan，但不像trunk,它不对承载的数据打标签。主要用于接入支持多vlan的服务器或者一些网络分析设备。现在基本不使用此类接口，在cisco的网络设备中，也基本不支持此类接口了。
4、dot1q-tunnel: 用在Q-in-Q隧道配置中。 
  
1、switchport mode access: 强制接口成为access接口，并且可以与对方主动进行协商，诱使对方成为access模式。 
  
2、switchport mode dynamic desirable: 主动与对协商成为Trunk接口的可能性，如果邻居接口模式为Trunk/desirable/auto之一，则接口将变成trunk接口工作。如果不能形成trunk模式，则工作在access模式。这种模式是现在交换机的默认模式。 
  
3、switchport mode dynamic auto: 只有邻居交换机主动与自己协商时才会变成Trunk接口，所以它是一种被动模式，当邻居接口为Trunk/desirable之一时，才会成为Trunk。如果不能形成trunk模式，则工作在access模式。   
4、switchport mode trunk: 强制接口成为Trunk接口，并且主动诱使对方成为Trunk模式，所以当邻居交换机接口为trunk/desirable/auto时会成为Trunk接口。
