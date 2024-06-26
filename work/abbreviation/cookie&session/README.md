拿flask实现举例：
1）首次（未登录）访问受限网站时，cookie是空的（cookie是存在客户端浏览器的，若有则每次请求时请求头要带"Cookie: k1=xx; k2=yy; k3=zz"，chrome可以在开发者工具中的application中管理存储的cookie）；
2）服务端使用flask.session.get(<id>)来获取该请求上下文传过来的cookie信息，显然获取不到，此时应该拒绝访问&跳转登录页面；
3）登录校验成功时，服务端使用flask.session[<id>] = <value>;把会话信息写入该请求上下文，然后本次服务端响应头会有"Set-Cookie: session=encode({<id>:<value>})"；
4）客户端收到响应头后会把内容保存下来，后续访问的请求头会有"Cookie: session=encode({<id>:<value>})"，这时候服务端执行步骤（2）时就能获取到信息就允许访问；

总之cookie是存储在客户端浏览器的（一组键值对），通过服务端的"Set-Cookie"响应头来设置的，有了之后每次请求会带上"Cookie"请求头；
而session是一种技术，服务端有很多方法来实现“会话控制”，具体要看服务端的代码是怎么写的，flask会往cookie中设置一个键名叫session的键值对；
cookie的几个属性：
1）path：形如"/xxx"，只有请求的path包含匹配path时，才会在请求头中带上这个cookie，例如，请求/xxx时，会带上"/"，"/xxx"两种path的cookie，但是不会带上"/yyy"这种cookie；
2）secure：只有https才会带上这个cookie，http认为不安全不带；
3）httponly：设置true时，浏览器会禁止javascript脚本读取该cookie

flask中相关接口：
flask.request.args.get('a')  # 获取请求http://xxx/yyy?a=3中的a参数值
flask.request.cookies  # 获取全部cookie信息
resp = flask.Response("hello p1")
resp.set_cookie(key='a', value='v', max_age=9999999)  # max_age是秒级别的ttl，到期后浏览器会自动删除，响应只有增量键值对，浏览器仍会保留历史其他键值对，响应效果是Set-Cookie: a=v; Expires=Thu, 25 Jul 2024 23:40:10 GMT; Max-Age=9999999; Path=/；参数说明：(max_age: 'timedelta | int | None' = None, expires: 'str | datetime | int | float | None' = None, path: 'str | None' = '/', secure: 'bool' = False, httponly: 'bool' = False)  
resp.delete_cookie('a')  # 响应效果是Set-Cookie: a=; Expires=Thu, 01 Jan 1970 00:00:00 GMT; Max-Age=0; Path=/
flask.request.cookies.get('a')
flask.request.files.get().save()
flask.session['username'] = '123' # 修改（添加）本次请求过来的session信息，响应头中会有Set-Cookie，进而达到修改客户端cookie的效果
flask.session.get('username') # 从本次请求过来的session信息中获取某<id>的值
flask.session.pop('username') # 修改（删除）本次请求过来的session信息
flask.session.clear() # 清空本次请求过来的session信息，进而达到清空客户端cookie的效果

Q1:
怎么设置otp（终极方案otp + 设备(ip)验证）

