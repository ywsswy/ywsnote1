Vtp server/client/transparent    ；设置VTP域的模式
//s1
Switch(vlan)#vtp server
Device mode already VTP SERVER.
Switch(vlan)#
//s2
Switch(vlan)#vtp client
Setting device to VTP CLIENT mode.
Switch(vlan)#
YWSS1(config)#int f0/1
YWSS1(config-if)#switchport mode access
YWSS1(config-if)#switchport access vlan 2

