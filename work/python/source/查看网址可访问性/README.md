import urllib.request
opener = urllib.request.build_opener()
opener.addheaders = [('User-agent', 'Mozilla/49.0.2')]
aa = ['http://www.baidu.com',
      'http://jw.liuhao.bid',
      'http://jwzx.cqupt.congm.in']
print('开始检查：')
for a in aa:
    tempUrl = a
    try:
        opener.open(tempUrl)
        print(tempUrl + '没问题')
    except urllib.error.HTTPError:
        print(tempUrl)
        print("urllib.error.HTTPError")
    except urllib.error.URLError:
        print(tempUrl)
        print("urllib.error.URLError")
    except:
        print("unknown exception")
