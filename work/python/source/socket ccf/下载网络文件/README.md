# 下载url文件，存储到本地
import urllib.request
import json
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
urllib.request.urlretrieve('http://ywsswy.cn:83/static/css/yIwannasee.css?v8', 'E:/Soft/tt.css')
