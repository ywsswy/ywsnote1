import urllib.request
import json
data_list = []
while True:
    try:
        temp = input()
        data_list.append(temp)
    except EOFError:
        break
data = json.dumps(data_list)
data = 'input={}'.format(data)
data = data.encode('utf-8')
url = 'http://106.53.5.24:83'
req = urllib.request.Request(url) 
req.add_header('Content-Type', 'application/x-www-form-urlencoded;charset=utf-8')
res = urllib.request.urlopen(req, data).read()
print('wrong')
