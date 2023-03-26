```
<link rel="stylesheet" type="text/css" href="外部引用的.css" />

<style type="text/css">
.alert { border:1px solid red; padding:5px; display:inline-block; }
text-transform:uppercase; color:red; }
</style>

## 示例
selector {property:value;}
/* comment */


（元素：E F）
（值：V）（class值：C）（id值：D）#id class的命名，根据功能来，不以样式来命名，最好小字母短线相连
（属性名：A）
（数字：N）
【简单选择器
*	*{}
E	p{}
#D	#logo{}	<h1 id="logo">
.C	.talk{}	<p class="talk">
E.C     p.talk{} <p class="talk">
【合并选择器
E1,E2	列表	h1,h2,h3{}
.V1.V2	混合	p.alert.critical	<p class="alert critical">
【组合器
E1 E2   div a   #两个选择器是血缘关系，E2是核心（被选中的那个）
E1>E2   div>a   #父子关系，E2是核心
E1+E2   div+a   #相邻兄弟，E2是核心
E1~E2   div~a   #可不相邻兄弟，E2是核心
【属性选择器
[A] 有属性A的元素
E[A]
[A="V"] 等于
[A~="V"] 属性A的值有多个（空格分割），其中一个是V
[A^="V"] 开头
[A$="V"] 结尾
[A*="V"] 包含
【其他
:hover 鼠标悬停的元素 li:hover{background-color: #ccc;}
:active 鼠标激活（按下未抬起）的元素
:focus 表单获得焦点的元素
E :nth-child(N) 父节点E的第N个子节点（1s）
E:nth-child(N) 并列的几个E的父节点的第N个E（n取值可为odd、even、3n+1）

```
border
padding
display
字体颜色
color:red
大写
text-transform:uppercase
1px solid red
inline-block

