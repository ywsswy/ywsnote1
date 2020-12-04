CVE
xss 是当用户访问一个被内嵌了恶意代码的网站时，即使这个网站是官方网站，比如qq空间，还是会发送此网站的cookie给黑客
如果我是网站开发者
在cookie中设置了HttpOnly属性，那么通过js脚本将无法读取到cookie信息，这样能有效的防止XSS攻击，具体一点的介绍请google进行搜索
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

