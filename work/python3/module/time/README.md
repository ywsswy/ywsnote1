a = time.time() #  取得当前时间的时间戳，float（整数部分是秒钟）
b = time.localtime(a)  # 将时间戳转为时间结构体，不写a就是当前时间
c = time.strftime('%Y-%m-%d %H:%M:%S', b)  # 将结构体可视化成字符串

d = time.strptime('20230312', "%Y%m%d")  # 将字符串转为结构体
e = time.mktime(d)  # 将结构体转为时间戳（可以加减法得出其他时间的时间戳）


##

time.sleep(<second>)

##

localtime(...)
    localtime([seconds]) -> (tm_year,tm_mon,tm_mday,tm_hour,tm_min,
                              tm_sec,tm_wday,tm_yday,tm_isdst)
    
    Convert seconds since the Epoch to a time tuple expressing local time.
    When 'seconds' is not passed in, convert the current time instead.

