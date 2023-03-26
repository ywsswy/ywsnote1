不要关闭
S1(config)#ip default-gateway 192.168.0.254
S1(config)#int vlan 1
S1(config-if)#ip address 192.168.0.8 255.255.255.0
S1(config-if)#no shut
S1(config-if)#
//////////////////////////////////////////////////////////////////////
路由器端口通常默认是shutdown的，交换机一般没有。所以配置完成后都需要启用这个端口，不然接上网线端口不亮无法使用。
