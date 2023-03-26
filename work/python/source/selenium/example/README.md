import selenium.webdriver
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities
def StartWeb():
	option = selenium.webdriver.ChromeOptions()
	option.add_argument('headless')
	d = DesiredCapabilities.CHROME
	d['loggingPrefs'] = {'browser': 'ALL'}
	myweb = selenium.webdriver.Chrome(chrome_options=option,desired_capabilities=d)
	myweb.maximize_window()
	myweb.get("http://localhost:92/?lng_s=116.380967&lat_s=39.913285&lng_e=116.384374&lat_e=39.913068")
	return myweb
def main():
	import time
	myweb = StartWeb()
	myweb.execute_script('''
console.log('nihao');
''')
	a = myweb.get_log('browser')
	print(a)
	# time.sleep(20)
	myweb.quit()
main()

