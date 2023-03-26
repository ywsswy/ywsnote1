///////////////////////////////
<标签 属性1="参数1">//字符串"
	=参数2//数字
【首级标签】
html
【一级标签】
	【二级标签】
	-	【属性】
		=	【参数】
head//内为不可见元素
	title//标题
	meta //无尾
	-	charset//编码格式
		=	utf-8
	script
	-	type
		=	text/javascript
		src//
		=	one.js//~
	link
	-	ref//规定当前文档与被链接文档之间的关系
		=	stylesheet//文档的外部css样式表
		type
		=	text/css
		href
		=	style.css
body
	header
	article
	aside//article里为附属信息；外为附加侧边栏之类的
	hl
	h2
	p
	b//文字加粗
	i//文字倾斜
	del//文字删除线
·	strong
	wbr//软换行位置//notear
	bobr//禁止换行，内可放wbr
	hr//没有成对的结束标签<=>无尾notear
	script
	input//notear
	-	type
		=	button
		value
		=	按钮
		onclick//点击事件
		=	alert('welcome!')
	a
	-	href
		=	http://www.baidu.com
		target
		=	_blank
		title
		=	此为标题
		class
		=	table_border
			table_hover//各个样式之间用空格分隔
	table
	-	border	//边框粗细
		=	1
		cellspacing	//单元格间距
		cellpadding	//单元格衬距（格内文字与格边框距离）
		width
		=	"100%"
			???????????/
		height
		bgcolor
		background
		bordercolor
		boedercolorlight//亮边框（左、上两边）颜色，当border不为0时设置此值有效
	nav//内放一块，内容为导航功能的连接
	span//内放东西，带有某属性
	dl//定义列表
		dt//项目
		dd//项目描述
			ul
				li
	img
	-	src	地址
		alt	图片不可见时的说明文字
	iframe(行内框架，放个盒子）
	-	src	
		width
		=	300
		height
		=	150
		name//当iframe外某处a的target属性设置为该name的值时，点击a会在这个iframe中打开
【常用属性】
-	align	=left
		right
		center
-	bgcolor	=red
		#00ff00
-	background=//背景图像
-	id	=
-	class	=
-	src	=http://9926345.s21i-9.faiusr.com/4/ABUIABAEGAAg44P6ugUoyby0wQUw_AM47wM.png
		file:///F|/fc/picture/Camera Roll/21.jpg
		
