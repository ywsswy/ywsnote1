反向代理，内网外入

https://ngrok.com/download
下载注册获取TOKEN

运行
ngrok authtoken <YOUR_AUTH_TOKEN>
会生成本地配置文件
C:\Users\Administrator\.ngrok2\ngrok.yml

编辑（Unix格式）

authtoken: 718Ca9RSUaYNRT76Dv3p4_E3828JjmCht1FSXJqjTx
tunnels:
  jwzx:
    proto: http
    addr: 202.202.32.202:80
  oa:
    proto: http
    addr: 202.202.43.137:80
  myftp:
    proto: http
    addr: 80

运行
ngrok start jwzx oa myftp
然后服务器会自动分配各自的外网地址映射



【other
https://ngrok.com/docs#config-location
web_addr

提供本地浏览器监视的Web界面和api
