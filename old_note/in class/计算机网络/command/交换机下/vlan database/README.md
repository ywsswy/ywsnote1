进入vlan 数据库
交换机的vlan信息全部是存在vlan 数据库里面的
你可以到flash里面看看 应该有个vlan.date的文件
cisco已经不推荐使用这个命令了，现在要创建vlan的话直接在conf t下打vlan就OK了（非也）
Switch#vlan database
% Warning: It is recommended to configure VLAN from config mode,
  as VLAN database mode is being deprecated. Please consult user
  documentation for configuring VTP/VLAN in config mode.
Switch(vlan)#
