
# XSS (Cross Site Script/跨站脚本攻击) 
在A网站的服务端数据库 or 客户端dom中“篡改”恶意代码，或者在A网站访问链接中嵌入恶意代码，当用户访问A网站时，代码获取到A网站的cookie发送给黑客；
防御）只能靠开发者，客户端用户再小心也难免中招；方法1：在cookie中设置了HttpOnly属性，那么通过js脚本将无法通过document.cookie读取到cookie信息


# CSRF (Cross-site request forgery/跨站请求伪造)
1）前提：用户登录了A网站，有cookie；
2）用户登录了B网页（某恶意网站，或者A网站中被“篡改”了的页面），网页的恶意代码（脚本或资源访问链接）中会请求A网站；（相当于用户在不知情的情况下访问了A网站的某接口，如果该接口是转账等接口，那么就会有财产损失）
防御）服务端开发不要添加"Access-Control-Allow-Origin"响应头，这时候通常现代浏览器就会进行拦截（当然可能有一些老旧的有漏洞的浏览器例外）；（同源的请求-不跨域除外，见下方）
如果服务端确实有需要放通的需求，可以添加"Access-Control-Allow-Origin"响应头，可以读取请求头中的Referer和Host字段来进行逻辑处理；
有安全风险的接口还应该使用更安全的验证身份的方法进行验证；
客户端用户小心使用不要访问不靠谱的网站；

ps）客户端有办法设置成关闭跨域拦截：
chrome完全关闭状态，设置启动快捷方式的属性，在exe后面加上 --args --disable-web-security --user-data-dir跨域参数
ie 在internet安全设置下，设置允许【跨域浏览窗口和框架】和【通过域访问数据源】

同源是指，域名，协议，端口相同。
比如说，下面几个是同源的：
http://example.com/
http://example.com:80/
http://example.com/path/file

而下面几个域名是不同源的：
http://example.com/
https://example.com/
http://example.com:8080/
http://www.example.com/



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