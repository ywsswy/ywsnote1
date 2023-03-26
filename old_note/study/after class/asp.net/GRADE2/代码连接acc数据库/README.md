Web.config
<?xml version="1.0" encoding="utf-8"?>
<configuration>
<appSettings>
<add key="AccessConnString" value="provider=microsoft.jet.oledb.4.0;data source="/>
<add key="AccessDbPath" value="~/db/guestbook.mdb"/>
</appSettings>
<connectionStrings>
<add name="AccessConnectionString" connectionString="Provider=Microsoft.Jet.Oledb.4.0;data source="/>
<add name="Access_Path" connectionString="~/db/guestbook.mdb"/>
<add name="SqlConnectionString" connectionString="Data Source=localhost;Initial Catalog=HuaRunDb;User ID=sa;password=123456;" providerName="System.Data.SqlClient"/>
</connectionStrings>
<system.web>
<compilation debug="true" targetFramework="4.0" />
</system.web>
</configuration>
后台 aspx.CS
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Web.Configuration;
using System.Configuration;
using System.Data.OleDb;
using System.Data;
namespace TestOracle
{
public partial class TestConnect : System.Web.UI.Page
{
public static readonly string connStr1 = System.Web.Configuration.WebConfigurationManager.ConnectionStrings["AccessConnectionString"].ConnectionString+ HttpContext.Current.Server.MapPath(WebConfigurationManager.ConnectionStrings["Access_Path"].ConnectionString);
public static readonly string connStr2 = System.Configuration.ConfigurationManager.AppSettings["AccessConnString"].ToString() + System.Web.HttpContext.Current.Server.MapPath(ConfigurationManager.AppSettings["AccessDbPath"]) + ";";
public static readonly string connStr3 = "Provider = Microsoft.Jet.OLEDB.4.0 ;Data Source=" + HttpContext.Current.Server.MapPath("~/db/guestbook.mdb");///////////////////
protected void Page_Load(object sender, EventArgs e)
{
}
protected void Button1_Click(object sender, EventArgs e)
{
OleDbConnection conn = new OleDbConnection(connStr1);
try
{
conn.Open();
string sql = "select * from GtContent";////////////////////////////////
OleDbDataAdapter myadapter = new OleDbDataAdapter(sql, conn);
DataSet ds = new DataSet();
myadapter.Fill(ds);
this.GridView1.DataSource = ds;
this.GridView1.DataBind();
this.Label1.Text = "数据库连接成功！";
}
catch (Exception ee)
{
this.Label1.Text = ee.ToString();
}
}
}
}
