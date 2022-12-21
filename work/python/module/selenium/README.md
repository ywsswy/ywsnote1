import selenium.webdriver
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities
def StartWeb():
    option = selenium.webdriver.ChromeOptions()
    option.add_argument('headless')
    d = DesiredCapabilities.CHROME
    d['loggingPrefs'] = {'browser': 'ALL'}
    myweb = selenium.webdriver.Chrome(chrome_options=option,desired_capabilities=d)
    # myweb.maximize_window()
    return myweb

def main():
    import time
    myweb = StartWeb()
    myweb.get("http://111.230.151.212:85")
    myweb.execute_script('''
console.log('huaerweishenme zheyanghong');
''')
    a = myweb.get_log('browser')
    print(a)
    myweb.quit()
main()
