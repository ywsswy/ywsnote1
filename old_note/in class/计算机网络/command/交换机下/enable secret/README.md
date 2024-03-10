设置交换机的特权密码（该密码以密文形式显示）
YwsSwitch2950-1(config)#enable secret Yws238894
YwsSwitch2950-1(config)#
//////////////////////////////////////////////////////////////////////
YwsSwitch2950-1#show run
Building configuration...
Current configuration : 1063 bytes
!
version 12.1
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname YwsSwitch2950-1
!
enable secret 5 $1$mERr$qol.2ZEpgwEBqEgPwST4Y/
enable password yws238894
