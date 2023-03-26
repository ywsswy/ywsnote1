# 跳过10次中的9次数据
num = 0
with open('E:/Soft/Dev-Cpp/yg.txt', 'r') as f:
    num = int(f.read())
    num += 1
with open('E:/Soft/Dev-Cpp/yg.txt', 'w') as f:
    f.write(str(num))
if int(num%10) != 0:
    exit()
import socket
import os
import json
obj = socket.socket()
obj.connect(('123.206.27.78', 12580))
name = 'E:/Soft/Dev-Cpp/yg.txt'
size = os.stat(name).st_size
data = {'size':size}
string = json.dumps(data)
obj.sendall(string.encode("utf-8"))
file_size_string = obj.recv(1024)
file_size = int(file_size_string)
if file_size == size:
    print("exist")
elif file_size != 0:
    print("continue")
with open(name, "rb") as f:
    f.seek(file_size)#断点续传
    for line in f:
        obj.sendall(line)
if obj.recv(1024) == "ok".encode('utf-8'):
    print('file send ok.')
obj.close()

