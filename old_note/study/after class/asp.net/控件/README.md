【TextBox】
MaxLanth 可以输入的最大字符长度
AutoPostBack自动提交
note：
代码的执行必须要在页面提交后才可以
【Panel】
//等价为div
//属于WebControl高级类，但Div属于LiteralControl低级类
//Div runat="server"属于HtmlControl类
【SelectConmmand】
//库.SelectCommand = "select * from list_newsclass where lnc_id > 1";
//SelectCommand="SELECT [lnc_id], [lnc_name] FROM [list_newsclass] WHERE ([lnc_id] = ?)"
//<SelectParameters>
//	<asp:QueryStringParameter DefaultValue="1" Name="lnc_id" QueryStringField="id" Type="Int32" />
//或者	<asp:ControlParameter ControlID="DropDownList1" Name="news_cid" PropertyName="SelectedValue" Type="Int32" />
//或者
//</SelectParameters>
//此参数可在浏览器地址后加上 ?id=2来修改
//具有记忆功能
//生效位置：PageLoad、控件事件、AccessDataSource.Selecting事件
【SelectParameters】
设置数据库参数：控件（级联查询、模糊查询，同一页面实时与用户交互）\QueryString（无二次更改SelectCommand的页面中）\DefaultValue
【GridView】
DataFormatString
//字段类型：绑定、特殊、按钮、命令、自定义
绑定：
文本、日期、数字货币
	日期格式：
		{0:d} 10/12/2002
		{0:D} December 10,2002或2002年12月10日
		{0:t} 10:11 PM
		{0:T} 10:11:29 PM
		{0:M} 7月10日
		{0:r} Tue, 10Dec 2002 22:11:29GMT
		{0:dd} 10//日
		{0:ddd} 周五
		{0:dddd} 星期五
		{0:yyyy} 2002
	货币：
		{0:c} Y44.33
		{0:c4} Y44.3300
自定义		{0:yyyy-MM-dd[dddd]}
string _s = string.Format("{0:ddd}",dt);//dt:日期时间变量

