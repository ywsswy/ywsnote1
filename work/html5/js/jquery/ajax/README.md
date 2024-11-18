希望跨域请求别人的话，怎么解决：
- 代理（搭建一个后台服务，在后台服务上访问页面然后返回来数据，并且添加响应头Access-Control-Allow-Origin: *）访问静态资源的时候万能，只不过数据传输链路比较长；
//使用ajax跨域请求代理服务端（ajax是个技术，不局限于jquery，只是说jquery内置了ajax的功能，如果不使用jquery，还可以使用var xhr = new XMLHttpRequest()来进行XHR2技术操作；或者使用fetch命令更现代原生）
$.ajax({url:"https://<my_backend_site>/?url=http://xxx/",success:function(result){
   console.log($(result).find('#1').eq(0).html());  // 请求xxx交给后台服务来请求，处理结果返回给前端，后端示例代码在下方
}});

希望允许别人跨域请求自己的话，怎么解决：
- 设置Access-Control-Allow-Origin: *
- 使用jsonp技术


from flask import Flask,render_template,request,Response
import urllib.request
import urllib.parse

app=Flask(__name__)
app.config.update({
    #'DEBUG':True,
    'TEMPLATES_AUTO_RELOAD':True
})

@app.route('/')
def home():
    r_query = request.args.get('url')
    url = r_query
    header = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36'}
    html_rq = urllib.request.Request(url, headers=header)
    html_json = urllib.request.urlopen(html_rq).read()
    resp = Response(html_json)
    resp.headers['Access-Control-Allow-Origin'] = '*'
    return resp

if __name__ == '__main__':
    app.run(port=80,host='0.0.0.0')





【eg 表单
        var pushType = $('#push').attr('my_data');
        var formData = new FormData($('#myForm')[0]);
        $.ajax({
            url:"/api?method=push",
            type: "POST",
            data: formData,
            async: true,
            cashe: false,
            contentType:false,
            processData:false,
            success:function (returndata){
                alert('成功');
                alert(returndata);
            },
            error: function (returndata) {
                alert('是白');
            }});
