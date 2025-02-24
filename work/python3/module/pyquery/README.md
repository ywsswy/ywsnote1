
from pyquery import PyQuery as pq


doc = pq(resc) #str to dom
print(doc('div.view_title').text())


### 依赖：lxml和cssselect
【eg
import urllib.request
from pyquery import PyQuery as pq
def yJudge(id):
    url = 'http://www.wlaqwk.com/view/{id}.html'.format(id=id)
    print(url)
    request = urllib.request.Request(url)
    request.add_header("Content-Type", "application/x-www-form-urlencoded;charset=utf-8")
    try:
        f = urllib.request.urlopen(request)
        res = f.read()
        #print(res)
        resc = res.decode('utf-8')
        #print(resc)
        doc = pq(resc)
        print(doc('div#view_title').text())
    except:
        #sys.stdout.write('\r')
        print('error')

def main():
    #from 80 to 1557
    #http://www.wlaqwk.com/view/1593.html
    for id in range(1535,1536,1):
        yJudge(id)
main()