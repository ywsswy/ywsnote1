拿flask实现举例：
1）首次（未登录）访问受限网站时，cookie是空的（cookie是存在客户端浏览器的，若有则每次请求时请求头要带"Cookie: xxxx"，chrome可以在开发者工具中的application中管理存储的cookie）；
2）服务端使用flask.session.get(<id>)来获取该请求上下文传过来的cookie信息，显然获取不到，此时应该拒绝访问&跳转登录页面；
3）登录校验成功时，服务端使用flask.session[<id>] = <value>;把会话信息写入该请求上下文，然后本次服务端响应头会有"Set-Cookie: session=encode({<id>:<value>})"；
4）客户端收到响应头后会把内容保存下来，后续访问的请求头会有"Cookie: session=encode({<id>:<value>})"，这时候服务端执行步骤（2）时就能获取到信息就允许访问；

总之cookie是存储在客户端浏览器的，通过服务端的"Set-Cookie"响应头来设置的，有了之后每次请求会带上"Cookie"请求头；
而session是一种技术，服务端有很多方法来实现“会话控制”，具体要看服务端的代码是怎么写的；

flask中相关接口：
flask.request.args.get('a')  # 获取请求http://xxx/yyy?a=3中的a参数值
flask.request.cookies.get()
flask.request.files.get().save()
flask.set_cookie/delete_cookie()
flask.session['username'] = '123' # 修改（添加）本次请求过来的session信息，响应头中会有Set-Cookie，进而达到修改客户端cookie的效果
flask.session.get('username') # 从本次请求过来的session信息中获取某<id>的值
flask.session.pop('username') # 修改（删除）本次请求过来的session信息
flask.session.clear() # 清空本次请求过来的session信息，进而达到清空客户端cookie的效果
