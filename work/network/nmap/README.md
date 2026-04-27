查看目标机器的端口/服务开放情况
nmap -T4 -A -v -Pn <ip>

>
Discovered open port 22/tcp on <ip>
Discovered open port 3389/tcp on <ip>

PORT     STATE SERVICE        VERSION
22/tcp   open  ssh            OpenSSH for_Windows_8.1 (protocol 2.0)
|_ssh-hostkey: ERROR: Script execution failed (use -d to debug)  ## 这种应该是没有配密钥登陆而是只能用密码登陆的意思
3389/tcp open  ms-wbt-server?