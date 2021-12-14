a = time.time() #取得当前时间的时间戳，float（整数部分是秒钟）
time_struct = time.localtime(a) #时间结构体
b = time.strftime('%Y-%m-%d %H:%M:%S',time_struct)
print(b)



##

time.sleep(<second>)

##

localtime(...)
    localtime([seconds]) -> (tm_year,tm_mon,tm_mday,tm_hour,tm_min,
                              tm_sec,tm_wday,tm_yday,tm_isdst)
    
    Convert seconds since the Epoch to a time tuple expressing local time.
    When 'seconds' is not passed in, convert the current time instead.


time.localtime(time.time())