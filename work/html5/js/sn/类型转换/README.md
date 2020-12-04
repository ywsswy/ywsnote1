parseFloat()方法的还有一不同之处在于，字符串必须以十进制形式表示浮点数

parseInt("0xA"); //returns 10
parseInt("22.5"); //returns 22
parseInt()方法还有基模式，能够把二进制、八进制、十六进制或其它不论什么进制的字符串转换成整数。基是由parseInt()方法的第二个參数指定的
parseInt("AF", 16); //returns 175
parseInt("10", 2); //returns 2

Boolean(value)——把给定的值转换成Boolean型；
Number(value)——把给定的值转换成数字（能够是整数或浮点数）；
String(value)——把给定的值转换成字符串。


var dic3 = { "zs": "张三1", "ls": "李四1", "ww": "王五1" };
var str1 = JSON.stringify(dic3)
JSON.parse(str1)
delete dic3['ww']
dic3['new'] = 'add'
var child1 = {'a':'aa','b':'bb'}
dic3['new2'] = child1 //此时修改child1，会影响dic3


python(json.dumps) --> js(JSON.parse)
dict->dict
list->Array

