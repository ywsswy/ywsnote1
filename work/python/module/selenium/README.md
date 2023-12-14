
.switch_to.window('BDDD9F73488A7464D972833216ED0D87')  # 切换tab页
.maximize_window() 最大化窗口
.execute_script("return $('textarea').get(0);")  # 获取DOM对象用于后续操作，注意不能用eq(0)，因为eq(0)获取的不是DOM对象，而是jquery对象，selenium无法操作jquery对象；
.send_keys("hello666");  # 对DOM对象进行操作，模拟键盘输入，如果不用selenium的方法而是用原生js的dispatchEvent或者jquery的val()和trigger()可能无法准确模拟；
.click()
.get_log('browser') 获取console日志？