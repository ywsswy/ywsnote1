jquery-1.7.2.js是开发板的，阅读起来比较方便，因为它的格式保留着，jquery-1.7.2.min.js是压缩版的，压缩版就是说它没有格式，其实内容和开发板的一样，只是不方便阅读
【query example
$('div[style^=margin-top]')
$('a:contains(删除)')
【attr
length			匹配到多少元素
【method
$('*').css('font-family','山自体');
remove()		删除显示元素
eq(0)			数组的第几个下标元素（0s）
addClass()
removeClass()
hasClass()		判断是否拥有
toggleClass()
removeAttr()
attr()			获取/设置 属性，获取yws.attr('id')，设置prop("checked",true)
prop()			获取/设置 元素本身带有的固有属性用prop方法,自定义的DOM属性用attr
val()			获取/设置 值，比如input/textArea的输入框，select下拉框的option的value
html()			返回或设置元素里面的全部innerHtml内容（如果内有标签则会有标签，不包括本标签），如果要outerHtml是prop('outerHTML')
find()			方法获得当前元素集合中每个元素的后代（是后代就行，不一定是儿子）
blur()			取消光标焦点
parent()                获取父元素
$(<str>)		字符串转jquery选择器
$(event.target)         类似于$(this)，event.target是dom元素，用这种方法转换为jquery对象，即获取到的是哪个元素触发的，且用event.target更好，因为this是冒泡的
.bind('click',function(event){/*do something*/}); 
unbind()                取消绑定

$.noConflict(true);//jQuery.noConflict(true);//$无效和jQuery都无效
$.noConflict();//jQuery.noConflict();//参数为空即为false
jQuery.noConflict(false);//$无效，jQuery依然有效
【动态加入console调试
var yws_add_jquery = document.createElement('script');
yws_add_jquery.src = 'https://code.jquery.com/jquery-1.11.0.min.js';
yws_add_jquery.type='text/javascript';
document.getElementsByTagName('head')[0].appendChild(yws_add_jquery);
【动态加入css
var yws_add_css = document.createElement('style');
yws_add_css.type = 'text/css';
yws_add_css.innerHTML='xxxx';
document.getElementsByTagName('head')[0].appendChild(yws_add_css);
【eg
$(".questionsArea dl").eq(0).attr('style','border: solid red 5px;');
#对匹配到的第一个（0s）元素 设置属性style的值，如果省略eq就是对全部匹配的都操作
一个选择器做不到就结合 eq和find来做
//////////////////////////////////////////////////////////////////////
$(".questionsArea dl").eq(1).find('dd').eq(0).find('span').eq(0).find('a').eq(0).removeClass('jqTransformChecked');
//////////////////////////////////////////////////////////////////////
var yRadio = $(".questionsArea dl a");//获取到一个数组
yRadio.each(function(a,b){//a是数组中元素下标（0s），b是该元素（yRadio[a]）
	if(a%5==0){
		$(b).click();//b.click()也可以，只要不使用jquery属性的时候
	}
})
//////////////////////////////////////////////////////////////////////
$(document).ready(function(){}) // 网页全加载完之后才会执行
等价于js中的
windows.onload = function{}
//////////////////////////////////////////////////////////////////////
$("a").trigger("click")就是触发a的click事件
$("a")[0].click();

【
$('#myForm')	    # 选择器对应的匹配元素集
$('#myForm').eq(0)  # 从选择器对应的匹配元素集中再精确地（根据索引）缩小（只取一个）匹配元素集
$('#myForm').eq(0)[0] # [0]直接把jquery包装的对象？=》js基本DOM对象（而不是str）

$('#myForm').eq(0)[0]==$('#myForm')[0]

true

$('#myForm').eq(0)==$('#myForm')

false


