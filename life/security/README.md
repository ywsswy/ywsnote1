## XSS & CSRF

XSS 是在目标网站上放恶意脚本等着用户访问。

CSRF 是在恶意网站上跨域向目标网站发出访问请求(例如<img href>)，特点是无需JavaScript且要求用户已经登陆过目标网站。

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