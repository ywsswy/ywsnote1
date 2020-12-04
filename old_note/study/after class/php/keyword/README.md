【结尾
PHP 语句以分号结尾（;）。PHP 代码块的关闭标签也会自动表明分号（因此在 PHP 代码块的最后一行不必使用分号）
【注释
// 这是单行注释
# 这也是单行注释
/*
这是多行注释块
它横跨了
多行
*/
【大小写 不！敏感
所有用户定义的函数、类和关键词（例如 if、else、echo 等等）都对大小写不敏感。
变量都对大小写敏感
【变量
变量以 $ 符号开头，变量名称必须以字母或下划线开头
变量会在首次为其赋值时被创建
类型松散，不必告知 PHP 变量的数据类型
#！！！！！！！！！双引号中的变量可以解析，单引号就是绝对的字符串
$a = '123';
$b = "456$a"; #456123     //这说明先解析$，后得到，不然应该转义$符号
var_dump($b);
$a = '123';
$b = '456$a'; #'456$a'     //这说明单引号是不解析的
var_dump($b);
【作用域
local（局部）
global（全局）
static（静态）
函数之外声明的变量拥有 Global 作用域，【只】能在函数以外进行访问。
函数内部声明的变量拥有 LOCAL 作用域，【只】能在函数内部进行访问。
（函数内部）变量前面使用 global 关键词，则全局变量就可见了
当函数完成/执行后，会删除所有变量。不过，有时我需要不删除则使用 static 关键词
【函数
function myTest() {
   echo "变量 x 是：$x";
} 
myTest();
【类型
查看类型gettype(); 或者 var_dump()；
integer
string	单引号或双引号
float
bool(true/false)
array
$x = null;
var_dump($x);#NULL
【class
class Car
{
  var $color;
  function Car($color="green") {
    $this->color = $color;
  }
  function what_color() {
    return $this->color;
  }
}

