from flask import Flask,url_for,Response,render_template
# 导入类

# 创建对象
app=Flask(__name__)
app.config.update({
    'DEBUG':True,
    'TEMPLATES_AUTO_RELOAD':True
})

# 装饰器 ，映射url中的/到这个函数上，那么当访问/目录时，会执行这个函数，返回值是返回给给浏览器的
# 默认methods = ['GET'],此时用户无法post
@app.route('/')
def hello_world():
    #return 'hello world(new1)'
    #return url_for('article_list')
    #return Response(response='hello')
    content = {
        'a': 3,
        'b': 'h',
        'info':{
            'name':'yws',
            'age':20
        }
    }
    return render_template('index.html',**content)


# /list/和/list在seo优化中是不同的，写斜杠能被两种都识别
@app.route('/list/')  # 默认methods方法是['GET']，可以设置['GET','POST']
def article_list():
    return 'my_list'


#
@app.route('/p/<int:article_id>/')
def article_detail(article_id):
    return r'''您请求的文章是{}'''.format(article_id)


# port默认5000，可以指定
if __name__ == '__main__':
    app.run(port=5200)








