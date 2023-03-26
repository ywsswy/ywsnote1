可以下载文件
import urllib.request

urllib.request.urlretrieve('http://ywsswy.cn:83/static/css/yIwannasee.css?v8', 'C:/ccf/tt.css')

【eg post
import urllib.request
data = 'xh=201521&sfzh=21'
data = data.encode('utf-8')
req = urllib.request.Request('http://jwc.cqupt.edu.cn/showStuQmcj.php')
req.add_header("Content-Type","application/x-www-form-urlencoded;charset=utf-8")
res = urllib.request.urlopen(req, data).read()#参数在参数中
print(res.decode('utf-8'))
【eg get
import urllib.request
req = urllib.request.Request('http://jwc.cqupt.edu.cn/showStuQmcj.php?xh=123') #参数在url中
req.add_header("Content-Type", "application/x-www-form-urlencoded;charset=utf-8")
#req.add_header('User-Agent','Mozilla/5.0 (Windows NT 6.1) (KHTML) Chrome')
res = urllib.request.urlopen(req).read()
print(res.decode('utf-8'))
【eg gzip解码
import urllib.request
import zlib
req = urllib.request.Request('http://jwc.cqupt.edu.cn/showStuQmcj.php')
res = urllib.request.urlopen(req).read()
data = zlib.decompress(res, zlib.MAX_WBITS|32) #响应数据可能是加密的，此时选择解密方式gzip/zlib
print(type(data))
print(data.decode('unicode-escape'))#bytes类型的unicode编码通过unicode-escape解码