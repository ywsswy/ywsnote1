//动态加入console调试
var jquery = document.createElement('script');
jquery.src = 'http://code.jquery.com/jquery-1.11.0.min.js';
document.getElementsByTagName('head')[0].appendChild(jquery);
//ajax跨域请求
$.ajax({url:"http://ywsswy.cn:86/?url=http://9icn.xyz/",success:function(result){
   console.log($(result).find('#1').eq(0).html());
   alert($(result).find('#1').eq(0).html());
}});


【跨域：
代理（后台访问页面然后返回来）
jsonp（同样需要后台配合使用）
XHR2（后台设置这个，允许被跨域访问）


我用的是flask后台访问，然后XHR2，接口给任何人调用
【2019年5月了解到flask可以设置任意跨域

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
    app.run(port=86,host='0.0.0.0')
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
