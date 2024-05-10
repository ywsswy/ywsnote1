【内网外入】：可以达到借助该台（能同时连内网和外网）机器，让外部网用户访问内网的效果

https://ngrok.com/download
下载注册获取TOKEN

运行
ngrok authtoken <YOUR_AUTH_TOKEN>
会生成本地配置文件
C:\Users\Administrator\.ngrok2\ngrok.yml

编辑（Unix格式）

authtoken: 718Ca9RSUaYNRT76Dv3p4_E3828JjmCht1FSXJqjTx
tunnels:
  jw:
    proto: http
    addr: 201.201.22.208:80
  oa:
    proto: http
    addr: 201.201.22.209:80
  myftp:
    proto: http
    addr: 80

运行
ngrok start jw oa myftp
然后服务器会自动分配各自的外网地址映射


【other
https://ngrok.com/docs#config-location
web_addr

提供本地浏览器监视的Web界面和api

【原理
以上三条命令类似建立了三个ssh远程端口转发；
理论上，这个内部完全可以“偷偷”建立多条远程端口转发隧道，转发到内网的多台机器的22端口，然后外网“攻击者”就可以暴力破解密码；