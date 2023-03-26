## SSL 和 SSH 区别
SSH是secure shell，通过此，可以安全登录控制台，可以安全传文件(SCP)
SSL是协议，Transport Layer Security (TLS)是SSL协议（Secure Sockets Layer）的升级版，TLS 1.0通常被标示为SSL 3.1，TLS 1.1为SSL 3.2，TLS 1.2为SSL 3.3
  client                  server
1）           ==>请求
2)           <==返回server_pub_key+CA证书
3)检查CA证书，选择是否通过认证继续访问
4)            ==>发送client_pub_key+CA证书+加密选择...
5)                         检查CA证书，选择是否通过认证继续访问
6)           <==返回加密方案（从这步开始有会话密钥用会话密钥，没有就用对方公钥）
7)            ==>发送会话密钥
8)           <==>互相使用会话密钥通信
* 为什么使用共享会话密钥（对称），因为这个加密解密速度快
