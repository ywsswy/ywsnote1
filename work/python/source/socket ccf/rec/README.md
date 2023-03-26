#导入SocketServer，多线程并发由此类实现
import socketserver
import json
import os
class mySocketServer(socketserver.BaseRequestHandler):
    def handle(self):
        print('Got a new connection from', self.client_address)
        clientport = str(self.client_address[1])
        while(True):
            try:
                string = self.request.recv(1024)
                #避免读到空字符串，程序报错
                if not string:   
                    break
                data = json.loads(string.decode("utf-8"))
                file_size = data['size']
                total_size = int(file_size)
                path = 'C:\\ccf\\getfile'
                has_recv = 0
                #向客户端发送当前以接收的文件大小，实现断点续传
                if not os.path.exists(path):
                    self.request.sendall("0".encode('utf-8'))
                else:
                    file_has_size = os.stat(path).st_size
                    self.request.sendall(bytes(str(file_has_size), encoding = 'utf-8'))
                    has_recv = int(file_has_size)
                #如果待上传部分为0，说明文件已经上传完毕
                if has_recv == total_size:
                    break
                f = open(path, 'ab')
                while(True):
                    if total_size == has_recv:
                        self.request.sendall("ok".encode('utf-8'))
                        break
                    data = self.request.recv(1024)
                    f.write(data)
                    has_recv += len(data)
                f.close()
            except ConnectionResetError:
                print("客户端关闭了连接。")
                break
            
if __name__ == '__main__':
#定义侦听本地地址口（多个IP地址情况下），这里表示侦听所有
    HOST = ''
#Server端开放的服务端口
    PORT = 12580
#调用SocketServer模块的多线程并发函数
    s = socketserver.ThreadingTCPServer((HOST, PORT), mySocketServer)
#持续接受客户端的连接
    s.serve_forever()
