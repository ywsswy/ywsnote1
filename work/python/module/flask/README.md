install pyOpenSSL & cryptography

flask_app_proxy = flask.Flask(__name__, static_folder=None)

- 1024以下的端口是privileged port，普通用户不能启动
- flask的session能力是设置一个key叫session的cookie，value格式形如：base64<data>.base64<create_time>.sha1；其中base64后的结尾的等于号会被去掉，data就是明文或者zlib(明文)的session业务信息，sha1是由data,create_time和SECRET_KEY运算的结果，所以使用不当有安全隐患：
- 【保护1】注意每次启动随机生成SECRET_KEY，flask_app_proxy.config.update({'SECRET_KEY': os.urandom(64)})，否则可以被破解：https://blog.csdn.net/m0_60716947/article/details/127395519
- 【保护2】秘密信息不要写进session中，因为session的第一段data内容是可以被base64_decode+zlib_uncompress分析的，SECRET_KEY只能保证session无法被伪造（内容和时间戳不被篡改），但是无法保证session内容不被解密，所以session中写一个用户id（用户名）就可以，不要写用户密码；
- 【保护3】设置session的过期时间（服务器判断，不是像普通cookie在客户端浏览器判断过期）flask_app_proxy.config.update({'PERMANENT_SESSION_LIFETIME': datetime.timedelta(minutes=30)})
- 设置session的cookie-path，避免不同path的SECRET_KEY不同会互相干扰，flask_app_proxy.config.update({'SESSION_COOKIE_PATH': "/path1"})

## 其他Q
- WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead
- run(threaded=True) # the process handle each request in a separate thread
- ssl安全验证 https://www.cnblogs.com/lykbk/p/ASDFQAWQWEQWEQWEQWEQWEQWEQEWEQW.html  # 有待测试，忘记了
- processes=True #这个最大会占cpu的核数个进程