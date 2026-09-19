# XSS (Cross Site Script/跨站脚本攻击) 
在A网站的服务端数据库 or 客户端dom中“篡改”恶意代码，或者在A网站访问链接中嵌入恶意代码，当用户访问A网站时，代码获取到A网站的cookie发送给黑客；
防御）只能靠开发者，客户端用户再小心也难免中招；方法1：在cookie中设置了HttpOnly属性，那么通过js脚本将无法通过document.cookie读取到cookie信息

# CORS（Cross-Origin Resource Sharing）是一种跨域访问机制
当浏览器开启了跨域安全保护策略时，跨域的请求<font color=red>并不会被浏览器阻止发出，也不会阻止请求的接受，仅仅是浏览器不会把响应的数据交给页面上的回调</font>而是抛出一个报错提示，所以跨域请求时服务端的逻辑已经执行了，仅仅是客户端响应时的拦截！！服务端可以通过"Access-Control-Allow-Origin"响应头来控制浏览器是否应该在响应时拦截；
ps）可能有一些老旧的有漏洞的浏览器还没有这个安全保护策略，而且客户端有办法设置关闭跨域拦截：
- chrome完全关闭状态，设置启动快捷方式的属性，在exe后面加上 --args --disable-web-security --user-data-dir跨域参数
- ie 在internet安全设置下，设置允许【跨域浏览窗口和框架】和【通过域访问数据源】
- 同源是指，域名，协议，端口相同。
比如说，下面几个是同源的：
http://example.com/
http://example.com:80/
http://example.com/path/file
而下面几个域名是不同源的：
http://example.com/
https://example.com/
http://example.com:8080/
http://www.example.com/

# CSRF (Cross-site request forgery/跨站请求伪造)
1）前提：用户登录了A网站，有cookie；
2）用户登录了B网页（某恶意网站，或者A网站中被“篡改”了的页面），网页的恶意代码（脚本或资源访问链接）中会请求A网站；（相当于用户在不知情的情况下访问了A网站的某接口，如果该接口是转账等接口，那么就会有财产损失）
防御）使用验证码：服务端有风险的逻辑要等待用户输入验证码（手机短信or邮件）后再执行；






## 技术要求
安全攻击技术、安全工具，理解安全漏洞产生原理及挖掘方法，有独立分析或挖掘经验。
熟悉服务器入侵检测、事件溯源、安全日志分析、能从数据中发现安全问题并提出解决方案
关注国内外最新安全攻防技术，关注最新的web漏洞和系统漏洞

1.http://www.cr173.com/soft/14313.html           
ida：反汇编（分析强）
2.http://www.52pojie.cn/thread-350397-1-1.html    
ollydbg：反汇编（调试强，脱壳用）
3.http://rj.baidu.com/soft/detail/15788.html?ald   
wireshark：抓包
4.http://www.pc6.com/softview/SoftView_55129.html  
010Editor：二进制
5.http://www.uzzf.com/soft/25803.html             
Stud_PE：辅助学习PE




## 界面劫持

两层页面，用户看到的顶层，点击却点击到底层


|---|标准名称|---|
|---|---|---|
|RSA2|SHA256WithRSA|强制要求RSA密钥的长度至少为2048|
|RSA|SHA1WithRSA|对RSA密钥的长度不限制，推荐使用2048位以上|


对公钥处理的数据，其填充内容为伪随机的16进制字符串，每次操作的填充内容都不一样。这就是为什么每次使用公钥加密数据得到的结果都不一样

rsa加密数据中不允许存在\0

RSA解密API
int RSA_private_decrypt(int flen, unsigned char *from, unsigned char *to, RSA *rsa, int padding)
参数说明：
flen: 解密密钥长度
from: 要解密信息
to: 解密后的信息
padding: 填充方式( RSA_PKCS1_PADDING ，RSA_PKCS1_OAEP_PADDING，RSA_SSLV23_PADDING，RSA_NO_PADDING)