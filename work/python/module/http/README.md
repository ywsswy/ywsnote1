简易自带服务器
python -m http.server --cgi 8081 #启动服务器，.\cgi-bin目录下
//////////////////////////////////////////////////////////////////////
#cgi-bin目录下的后台文件
import os
import cgi
header = 'Content-Type: text/html\n'
html = 'a'
print(header)
print(html)
getar = cgi.FieldStorage()
getad = {}
for i in getar.keys():
    getad[i] = getar[i].value
arguments = sorted(getad.keys())
with open('out.txt', 'a+') as f:
    for i in arguments:
        f.write('{}\n'.format(getad[i]))
//////////////////////////////////////////////////////////////////////
import urllib.request
data_list = []
while True:
    try:
        temp = input()
        temp = urllib.parse.quote(temp)
        data_list.append(temp)
    except EOFError:
        break
url_raw = 'http://127.0.0.1:8081/cgi-bin/1.py?'
index = 1
for i in data_list:
    url_raw = '{}a{}={}&'.format(url_raw,index,i)
    index = index+1
print(url_raw)
req = urllib.request.Request(url_raw) #参数在url中
req.add_header("Content-Type", "application/x-www-form-urlencoded;charset=utf-8")
#req.add_header('User-Agent','Mozilla/5.0 (Windows NT 6.1) (KHTML) Chrome')
res = urllib.request.urlopen(req).read()
print(res.decode('utf-8'))
