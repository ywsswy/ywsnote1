【jinjia2 html模板中怎么写
视图函数 return render_template('index.html')#可以返回templates文件夹下的模板文件
app = Flask(__name__,template_folder=r'C:\templates')#可以设置为另一个文件夹

【后台传递过来
content = {
        'a': 3,
        'b': 'h',
        'info':{
            'name':'yws',
            'age':20
        }
    }
    return render_template('index.html',**content)
【模板（html）中显示
{# ... #}：注释
{{ a }}变量a的值#如果没有a，则是空  内置函数？url_for 
{{ info.name }} 等于 {{ info['name'] }}
关于点.号访问和[]中括号访问，没有任何区别，都可以访问属性和字典的值。
{% ... %}：控制语句 for if 定义赋值set
1）
{% if ... %}
{% elif ... %}
{% else %}
{% endif %}

{% for .. in .. %}<!--遍历列表跟普通写法一样，遍历字典{% for k,v in ...items() %}-->
{{loop.index}}<!--这是jinja2 for循环中自带的变量-->
{% else %}<!--要遍历的列表/其他为空-->
{% endfor %}


{% set name='xiaotuo' %}
赋值语句创建的变量在其之后都是有效的，如果不想让一个变量污染全局环境，可以使用with语句来创建一个内部的作用域，将set语句放在其中，这样创建的变量只在with代码块中才有效，看以下示例：
{% with %}
    {% set foo = 42 %}
    {{ foo }}           foo is 42 here
{% endwith %}

过滤器
{{ name|length }}，将返回name的长度。过滤器相当于是一个函数，把当前的变量传入到过滤器中，然后过滤器根据自己的功能，再返回相应的值
{{ name|int }}
{{ name|string }}
{{ name|safe }} #不进行转义处理
【add
for中自带变量
| loop.index | 当前迭代的索引（从1开始） |
| loop.index0 | 当前迭代的索引（从0开始） |
| loop.first | 是否是第一次迭代，返回True或False |
| loop.last | 是否是最后一次迭代，返回True或False |
| loop.length | 序列的长度 |
for 可以反向遍历 |reverse


快捷键tab

逻辑运算       and, or, not
