教程
https://www.bilibili.com/video/BV1Z4411o7TA/?p=3&vd_source=043e9779eed286d25f55da209251818b

0）chrome
- ubuntu
https://bestim.org/chrome-114.html
- centos
https://www.cnblogs.com/ianduin/p/8727333.html
google-chrome-stable.x86_64   108.0.5359.124-1 

1）selenium库

2）chromedriver（仅图形化调试时需要）
if YGlobal.args.noUI:
        option = selenium.webdriver.ChromeOptions()
        option.add_argument('headless')
        d = DesiredCapabilities.CHROME
        d['loggingPrefs'] = {'browser': 'ALL'}
        # YGlobal.wd = selenium.webdriver.Chrome(chrome_options=option, desired_capabilities=d)  # centos
        YGlobal.wd = selenium.webdriver.Chrome(options=option)  # ubuntu/mac
else:
        option = selenium.webdriver.ChromeOptions()
        # option.add_experimental_option('debuggerAddress', '127.0.0.1:9666')  # 可以配置远程debug接口（这样就是可以直接操作已经人工打开的页面，而不是从空白浏览器开始），要求chrome是用--remote-debugging-port=9666参数启动的，localhost:9666/json可以获取每个tab页的id
        YGlobal.wd = selenium.webdriver.Chrome(
            service=selenium.webdriver.chrome.service.Service('/<path_to>/chromedriver'), options=option)

chrome114之后的driver下载：
https://googlechromelabs.github.io/chrome-for-testing/
chrome114及以前的driver下载：
https://chromedriver.storage.googleapis.com/index.html




