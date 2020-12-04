//deal_* 鼠标键盘操作
//main_* http控制，调用：deal_*
Dim time1
time1 = 222
time2 = 444
Function deal_first(company, name, mail, user, pwd)
	//开始时，浏览器应处于最顶层，招聘页面
	MoveTo 55, 130 //Pnone
	Delay time1
	KeyPress "Home", 1
	Delay time1
	LeftClick 1
	Delay time1
	KeyPress "Tab", 1
	Delay time1
	KeyPress "BackSpace", 1
	Delay time1
	SayString company
	Delay time1
	MoveTo 452, 244 //P1
	Delay time1
	LeftClick 1
	Delay time1
	MoveTo 447, 273 //P2
	Delay time1
	LeftClick 1
	Delay time1
	MoveTo 544, 269 //P3
	LeftClick 1
	KeyPress "Tab", 1
	Delay time1
	SayString name
	Delay time1
	KeyPress "Tab", 1
	Delay time1
	KeyPress "Tab", 1
	Delay time1
	KeyPress "Tab", 1
	Delay time1
	SayString mail
	Delay time1
	KeyPress "Tab", 1
	Delay time1
	SayString user
	Delay time1
	KeyPress "Tab", 1
	Delay time1
	SayString pwd
	//结束时页面处于顶部，手机输入框无遮挡，可以有旧手机
End Function
Function deal_second(phone)
	//开始时页面处于顶部，手机输入框无遮挡，可以有旧手机
	MoveTo 461, 319 //P6
	Delay time1
	LeftDoubleClick 1
	Delay time1
	SayString phone
	Delay time1
	MoveTo 557, 352 //P4
	Delay time1
	LeftClick 1
	Delay 3333
	MoveTo 498, 367 //P5
	Delay time1
	RightClick 1
	Delay time2
	KeyPress "V", 1
	Delay 2222
	SayString "1.png"
	Delay time1
	KeyPress "Enter", 1
	Delay time1
	KeyPress "Y", 1
	Delay 1111
	KeyDown 16, 1
	Delay time1
	KeyPress "Tab", 1
	KeyUp 16, 1
	//结束时光标应该停在图片验证码输入框
End Function
Function deal_third(code)
	//开始时光标应该停在图片验证码输入框
	SayString code
	Delay time1
	KeyPress "Tab", 1
	Delay time1
	KeyPress "Enter", 1
	Delay 333
	//结束时首页顶部
End Function
Function deal_fourth(mess)
	//开始时首页顶部
	MoveTo 457, 351 //P7
	Delay time1
	LeftDoubleClick 1
	Delay time1
	SayString mess
	Delay time1
	KeyPress "Tab", 1//Tab * 7
	Delay time1
	KeyPress "Tab", 1
	Delay time1
	KeyPress "Tab", 1
	Delay time1
	KeyPress "Tab", 1
	Delay time1
	KeyPress "Tab", 1
	Delay time1
	KeyPress "Tab", 1
	Delay time1
	KeyPress "Tab", 1
	Delay time1
	KeyPress "Enter", 1
End Function
Function checkType(var,right_type)
	If typename(var) <> right_type Then 
		TracePrint "error,var: " & var & ", var_type: " & typename(var) & ", require_type: " & right_type
		ExitScript
	End If
End Function
Function httpget(url)
	checkType url, "String"
	bufres = Lib.旋_WINHTTP.高级_网页访问Ex(url, 0, "", "", "", False, "", "", "", "", 500, "UTF-8")
	httpget = bufres(1)
End Function
Function main_first
	//填写基本信息
	res_company = httpget("http://123.206.27.78:89/state_a?type=company")
	Delay 111
	res_name = httpget("http://123.206.27.78:89/state_a?type=name")
	Delay 111
	mail_rand = Lib.算法.随机数字串(15) & "@163.com"
	res_user = httpget("http://123.206.27.78:89/state_a?type=user")
	Delay 111
	res_pwd = httpget("http://123.206.27.78:89/state_a?type=pwd")
	TracePrint res_company & ", " & res_name & ", " & mail_rand & ", " & res_user & ", " & res_pwd
	deal_first res_company,res_name,mail_rand,res_user,res_pwd
End Function
Function main_second
	ant_token = "32abd72da67e95f2884119f42d506129"
	ant_url_release = "http://www.66yzm.com/api/admin/releaseadd?linpai=" & ant_token
	
	Rem RESTARTPHONE	
	res_release = httpget(ant_url_release)
	// TracePrint res_release //14:释放成功
	Delay 2222
	ant_url_phone = "http://www.66yzm.com/api/admin/getmobile?linpai=" & ant_token & "&itemid=941"
	res_phone = httpget(ant_url_phone)
	If len(res_phone) <> 13 Then 
		TracePrint "手机号获取失败？mess: " & res_phone
	End If
	ant_phone = Mid(res_phone, 2, 11) 
	deal_second ant_phone
	Delay 1111
	Rem STARTOCR
	res_ocr_start = httpget("http://123.206.27.78:92/get_ocr?type=start") //让服务器2清空旧记录，重新识别1.png
	//应该res_ocr_start = 'ok'
	res_ocr = ""
	httpget("http://123.206.27.78:89/sent_info?info=code_wait_OCR")
	Delay 8555
	res_ocr = httpget("http://123.206.27.78:92/get_ocr?type=get")
	TracePrint "OCR is: " & res_ocr
	httpget ("http://123.206.27.78:89/sent_info?info=OCR_is" & res_ocr)
	If len(res_ocr) = 0 Then 
		Goto STARTOCR
	End If
	deal_third res_ocr
	ant_url_mess = "http://www.66yzm.com/api/admin/shortmessage?linpai=" & ant_token & "&itemid=941&mobile=" & ant_phone
	TracePrint ant_url_mess
	res_mess = ""
	start_time = Now
	While res_mess = ""
		now_time = Now
		int_time = DateDiff("s", start_time, now_time)
		If int_time > 70 Then 
			TracePrint "重新开始下一个手机"
			Goto RESTARTPHONE
		End If
		Delay 7777
		res_mess = httpget(ant_url_mess)
		TracePrint res_mess // 17：没收到
		If res_mess = "17" Then 
			httpget("http://123.206.27.78:89/sent_info?info=phone_wait_message" & ant_phone)
			res_mess = ""
		End If
	Wend
	res_mess_code = Mid(res_mess, 33, 6)
	TracePrint res_mess_code
	deal_fourth res_mess_code
End Function
Function main
	now_info = "wait_start"
	While 1 = 1
		res_sent = httpget("http://123.206.27.78:89/sent_info?info=" & now_info)
		TracePrint res_sent
		If res_sent = "start" Then 
			now_info = "start_fill_basic_info"
			res_sent = httpget("http://123.206.27.78:89/sent_info?info=" & now_info)
			TracePrint res_sent
			main_first
			
			now_info = "start_phone_and_code"
			res_sent = httpget("http://123.206.27.78:89/sent_info?info=" & now_info)
			TracePrint res_sent
			main_second 
			
			now_info = "wait_start"
			res_sent = httpget("http://123.206.27.78:89/sent_info?info=" & now_info)
			TracePrint res_sent
			TracePrint "等待tempermonkey开启下一家公司"
			Delay 22223
		End If
		Delay 9999
	Wend
end function
delay 1100
main
Delay 1100
Function comment
//mess1 = Plugin.Sys.GetCLB
//1280x720 页面可以拉到右侧无黄色
//50%缩放页面
//Pnone = 网页左上角
//P1 = 省
//P2 = 省选择
//P3 = 市选择
//P4 = 获取验证码按钮(下方要有下载）
//P5 = 验证码图片(下方要有下载）
//P6 = 手机号输入框(下方要有下载）
//P7 = 短信验证码输入框(下方要有下载）
End Function
