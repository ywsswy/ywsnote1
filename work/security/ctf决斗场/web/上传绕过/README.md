【00截断
上传漏洞主要就是利用'\0'字符漏洞，'\0'字符在系统中被用作字符串结束的标志
基本原理就是网络程序虽然禁止了.asp等后缀名文件的上传，但是都会允许.jpg或者.gif格式文件的上传
此题目的：
上传成功：文件后缀jpg才成功
保存木马到服务器：php才成功
path="upfiles/picture/" 
file="20121212.jpg" 
upfilename=path+file
如果我们在path中存在一个00截断，最终保存的文件就是00之前的路径了
所以burp获取，在upload路径中加入00截断（hex中改）
flag{SimCTF_huachuan} 

