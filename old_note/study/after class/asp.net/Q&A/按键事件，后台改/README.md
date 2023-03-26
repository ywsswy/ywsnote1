////////////////////////////////////////
<form  class="form form-horizontal" id="form1" runat="server" action="?Action=edi">
<asp:Button ID="Button1" class="btn btn-primary radius" name="third" runat="server" OnClick="Button1_Click" Text="保存更改" />
后台判断是第一次访问的刷新，还是按键的提交
protected static string sid = "";//要静态
YAlert a1 = new YAlert();
string Action = "";//要动态
void Page_Load(object sender, EventArgs e){
	if (!string.IsNullOrEmpty(Request.QueryString["Action"])){
		Action = Request.QueryString["Action"].Trim().ToLower();//去掉空格并变小写
	}
	if (Action != "edi"){
		//初次访问进入，否则跳去其他事件
		OleDbConnection conn = new OleDbConnection(connStr1);
		conn.Open();
		sid = Request["id"];
		string snum = "select * from tCow where Id = " + sid;
		OleDbCommand cmd2 = new OleDbCommand(snum, conn);
		OleDbDataReader re = cmd2.ExecuteReader();
		re.Read();
		TextBox1.Text = re.GetValue(1).ToString();
		TextBox2.Text = re.GetValue(2).ToString();
	}
}
protected void Button1_Click(object sender, EventArgs e){
	YAlert a1 = new YAlert();
	a1.Alert(sid);
	OleDbConnection conn = new OleDbConnection(connStr1);
	conn.Open();
	string s1 = TextBox1.Text;
	string s2 = TextBox2.Text;
	string snum = "update tCow set cowName = " + s1 + ",cowEqu = " + s2 + " where Id = " + sid;
	OleDbCommand cmd2 = new OleDbCommand(snum, conn);
	cmd2.ExecuteNonQuery();
}
