网页相应数据可能是gzip加密的
【eg
import urllib.request
import zlib
    request = urllib.request.Request('http....')
    f = urllib.request.urlopen(request)
    data = f.read()
    data = zlib.decompress(data, zlib.MAX_WBITS|32)
    data = data.decode('unicode-escape')

