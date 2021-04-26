【1 网页传输/响应汉字
http://jwgl.cust.edu.cn/teachwebsl/login.aspx
（aspx utf-8）“用”：“0xe794a8”（3个字节，wireshark抓包）
http://111.230.151.212:83/
（python-flask utf-8）“刷”：“0xe588b7”（python-flask的默认return）
（高德json-api utf-8）
【2 url域 & post_data域（form的默认）格式是<key>=<value>，其中的中文字符
http://111.230.151.212:83/
（utf-8）：“革”：“0x254539253944254139”（9个字节，ASCII对应的打印字符是%E9%9D%A9）
python_url域 使用urllib.parse.quote('革') # 参照cclg
python_post_data域 使用'chat={}'.format(urllib.parse.quote('运行')).encode('utf_8')
【3
C++代码构造post/url请求域
C++编解码

【others
# 如何把wireshark中的http字节信息，转换成可以显示的html文件
* Unicode字符编码表
https://blog.csdn.net/zhenyu5211314/article/details/51537778
* post和get的区别
https://blog.csdn.net/qq_26360877/article/details/70665820
* python官网的各种编码名称
https://docs.python.org/3.6/library/codecs.html#standard-encodings
* 不要想显示的问题（控制台代码页），只管存储文件，人家自己tail去看吧
```
f = open('1.txt', 'rb')
f.read().decode(encoding='utf-8')
```
* notepad++是不值得信任的，其中如果文件是GB2312编码，其HexEdit中依然显示出utf-8的样子，所以需要手动重新建立一个utf-8编码的文件，同时应该在vim中查看:%!xxd :set fileencoding
* 
```
#!/usr/bin/env python3
同时确保文本编辑器正在使用UTF-8 without BOM编码
```
# 【“运”的三种编码
1)
0x8FD0 js-unicode/计算机内存 
'运'.charCodeAt(0) ord('运')
2)
0xD4CB ANSI存储（ANSI编码是一种对ASCII码的拓展）/GB2312存储(cp936)
char str[4] = "运";string str2 = "运";'运'.encode('ANSI')
3)
0xE8BF90 utf-8存储/网络传输
'运'.encode('utf-8')
# 
编码：的意思是字符集中的字符（如unicode字符）按照编码方式（如UTF-8）编成字节序列“传输”或“存储”到计算机中，因为一般认为编码方式可以变长节省空间。
解码：就是把字节序列，以当初它编码的规则转换回字符集，“显示”出来

【python
type('运') # str
'\u8fd0' == '运' # True
len('\u8FD0') # 1

encode(self, /, encoding='utf-8', errors='strict') #把计算机内存str转为供传输储存的bytes，配对decode
# encoding参照https://docs.python.org/3.6/library/codecs.html#standard-encodings
'运'.encode('utf-8') # b'\xe8\xbf\x90'
len(b'\xe8\xbf\x90') # 3

	r_question = request.args.get('question')	# 已经自动unquote成了'%u56FD&%u6B4C'
        r_question = r_question.replace("%", "\\")	# '\u56FD&\u6B4C'
        r_question = r_question.encode('utf-8')		# b'\\u56FD&\\u6B4C'
        r_question = r_question.decode('unicode_escape')# '国&歌'