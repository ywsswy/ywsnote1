# 这个例子的背景是读取excel，把一些公司信息，逐条注册到一个招聘网站上，但是这个招聘网站注册页面除了填写信息，还需要验证码。
# 所以这里就用了selenium

import selenium.webdriver
#from selenium.webdriver.common.keys import Keys
import requests

# 这是一个打码 识别图片验证码的平台
class RClient(object):
    def __init__(self, username, password, soft_id, soft_key):
        self.username = username
        import hashlib
        self.password = hashlib.md5(password.encode('utf-8')).hexdigest()
        # print(self.password)
        self.soft_id = soft_id
        self.soft_key = soft_key
        self.base_params = {
            'username': self.username,
            'password': self.password,
            'softid': self.soft_id,
            'softkey': self.soft_key,
        }
        self.headers = {
            'Connection': 'Keep-Alive',
            'Expect': '100-continue',
            'User-Agent': 'ben',
        }
    def rk_create(self, im, im_type, timeout=60):
        """
        im: 图片字节
        im_type: 题目类型
        """
        params = {
            'typeid': im_type,
            'timeout': timeout,
        }
        params.update(self.base_params)
        files = {'image': ('a.jpg', im)}
        r = requests.post('http://api.ruokuai.com/create.json', data=params, files=files, headers=self.headers)
        return r.json()
    def rk_report_error(self, im_id):
        """
        im_id:报错题目的ID
        """
        params = {
            'id': im_id,
        }
        params.update(self.base_params)
        r = requests.post('http://api.ruokuai.com/reporterror.json', data=params, headers=self.headers)
        return r.json()
def StartWeb():
    myweb = selenium.webdriver.Chrome()
    myweb.minimize_window()
    myweb.get("https://rd5.zhaopin.com/custom/register?za=2&ps=1")
    myweb.execute_script('''
var yws_add_jquery = document.createElement('script');
yws_add_jquery.type='text/javascript';
yws_add_jquery.src = 'https://code.jquery.com/jquery-1.11.0.min.js';
document.getElementsByTagName('head')[0].appendChild(yws_add_jquery);
''')
    import time
    while True:
        try:
            time.sleep(2)
            myweb.execute_script('$("html").size();')
            print('include jquery ok')
            break
        except Exception as mye:
            print('once again{}'.format(mye))
    return myweb
#本地js引入
def ySimpleRequest(url):
    import urllib.request
    req = urllib.request.Request(url)
    res = urllib.request.urlopen(req).read()
    data = res.decode('utf-8')  # bytes类型的unicode编码通过unicode-escape解码
    return data
def GetPhone(ant_token):
    ant_url_release = 'http://www.66yzm.com/api/admin/releaseadd?linpai={}'.format(ant_token)
    import time
    while True:
        res_release = ySimpleRequest(ant_url_release)#14:释放成功
        #print(res_release)
        time.sleep(2)
        ant_url_phone = 'http://www.66yzm.com/api/admin/getmobile?linpai={}&itemid={}'.format(ant_token,941)
        ant_phone = ySimpleRequest(ant_url_phone)
        if len(ant_phone) == 13:
            print('获取到手机号：{}'.format(ant_phone[1:12]))
            return ant_phone[1:12]
        else:
            print('手机号获取失败？mess: {}'.format(res_phone))
    return None
def GetMess(ant_phone,ant_token):
    import time
    start_time = time.time()
    res_mess = ''
    while True:
        ant_url_mess = 'http://www.66yzm.com/api/admin/shortmessage?linpai={}&itemid={}&mobile={}'.format(ant_token,941,ant_phone)
        res_mess = ySimpleRequest(ant_url_mess)
        if res_mess == '17':
            print('获取短信中...')
            now_time = time.time()
            if (now_time - start_time) > 50:
                print('长时间未获取到短信，切换下一个手机号')
                return 'restart'
            time.sleep(6.5)
        else:
            print(res_mess) #
            return res_mess[32:38]
    
def FillPhoneAndCode(myweb,ant_phone,ant_token,rc):
    phone_ele = myweb.execute_script('return $("input[name=mobile]")[0];')
    phone_ele.clear()
    phone_ele.send_keys('{}'.format(ant_phone))
    while True:
        myweb.execute_script('return $("a.register-verify-code__send-button")[0];').click()
        import time
        time.sleep(1)
        import base64
        img_b64_data = myweb.execute_script('return $("img").eq(0).prop("src");')[22:]
        img_data = base64.b64decode(img_b64_data)  # base64解码
        res = rc.rk_create(img_data, 3040)
        try:
            res_code = res['Result']
        except:
            res_code = 'AAAA'
        print('图片验证码识别结果:{}'.format(res_code))
        myweb.execute_script('return $("input[name=v-code]")[0];').send_keys('{}'.format(res_code))
        import time
        time.sleep(1)
        try:
            myweb.execute_script('return $("button.k-button")[1];').click()
        except:
            print('can not click/kb')
            continue
        time.sleep(2)
        button_str = myweb.execute_script('return $("a.register-verify-code__send-button").eq(0).text();')
        try:
            button_str.index('秒')
        except:
            print('图片验证码识别错误，识别下一张二维码')
            continue
        break
def FillBasic(myweb,item_dict):
    import time
    print('开始填写注册表单')
    import random
    myweb.execute_script('return $("input[name=companyName]")[0];').send_keys(item_dict['company'])
    myweb.execute_script('return $("input[name=province]")[0];').click()
    time.sleep(1)
    try:
        myweb.execute_script('return $("span[title=北京]")[0];').click()
        myweb.execute_script('return $("span[title=北京]")[1];').click()
    except:
        None
    myweb.execute_script('return $("input[name=linkName]")[0];').send_keys(item_dict['name'])
    myweb.execute_script('return $("input[alias=email]")[0];').send_keys('{}@163.com'.format(''.join(random.sample('0123456789zyxwvutsrqponmlkjihgfedcba',9))))
    myweb.execute_script('return $("input[alias=userName]")[0];').send_keys(item_dict['user'])
    myweb.execute_script('return $("input[alias=password]")[0];').send_keys(item_dict['pwd'])
    print('完毕，开始获取手机号')
    
def main():
    ### 删掉了一些代码
        myweb = StartWeb()
        FillBasic(myweb,data_dict[now_id])
        while True:
            FillPhoneAndCode(myweb,ant_phone,ant_token,rc)
            if res_mess != 'restart':
                myweb.execute_script('return $("input[name=verifyCode]")[0];').send_keys('{}'.format(res_mess))
                break
        myweb.execute_script('return $("button.register-buttons__submit")[0];').click()
        print(myweb.execute_script('return $("div.register-item__validate--error").eq(0).text();'))
        myweb.quit()
main()

