# 搭简单服务，直接去githubcopy吧。。。。


You're trying to run the app on a privileged port (81) - if you use a higher port such as 5000 you won't need sudo privileges.

run(threaded=True) # the process handle each request in a separate thread
 #生产环境。。。会搞崩服务器的吗？
processes=True #这个最大会占cpu的核数个进程

## cookie是存在客户端，每次请求带过来，session是存在服务端

request.args.get()
request.cookies.get()
request.files.get().save()
set_cookie/delete_cookie()



from flask import Flask,session


app.config['SECRET_KEY'] = os.urandom(24)

session['username'] = '123'

session.get('username')

session.pop('username')

session.clear()


from datetime import timedelta
import os
app.config['PERMANENT_SESSION_LIFETIME'] = timedelta(days=7)


pip install pyOpenSSL

sudo pip install cryptography --upgrade
#################
ssl安全验证
https://www.cnblogs.com/lykbk/p/ASDFQAWQWEQWEQWEQWEQWEQWEQEWEQW.html
##############有待测试，忘记了