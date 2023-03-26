scheme://host:port/path/?query-string#anchor


# /list/和/list在seo优化中是不同的，写斜杠能被两种都识别
@app.route('/list/')  # 默认methods方法是['GET']，可以设置['GET','POST']
def article_list():
    return 'my_list'



# URL与函数的映射，可以指定URL的规则
# 尖括号是固定写法，语法为<converter:variable_name>
# converter指明类型
'''
string: 默认的数据类型，接受没有任何斜杠“\/”的文本。
int: 接受整形。
float: 接受浮点类型。
path： 和string的类似，但是接受斜杠。
uuid： 只接受uuid字符串。通用唯一识别码
any：可以指定多种路径<any(article,blog):url_path>

也可以通过传统的?=的形式来传递参数，
例如：/article?id=xxx，这种情况下，
可以通过request.args.get('id')来获取id的值。
如果是post方法，则可以通过request.form.get('id')来进行获取
# 通过问号的形式传递参数
import request
@app.route('/d/')
def d():
    wd = request.args.get('wd')
    ie = request.args.get('ie')
    print('ie:',ie)
    return '您通过查询字符串的方式传递的参数是：%s' % wd
'''


@app.route('/p/<int:article_id>/')
def article_detail(article_id):
    return r'''您请求的文章是{}'''.format(article_id)


