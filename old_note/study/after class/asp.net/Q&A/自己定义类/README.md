/////////////////////////////
定义：
在网站根目录App_Code下建立YAlert.cs
namespace YClass
{
    /// <summary>
    /// YAlert 的摘要说明
    /// </summary>
    public class YAlert
    {
        public YAlert()
        {
            //
            // TODO: 在此处添加构造函数逻辑
            //
        }
        public void Alert(string msg)
        {
            System.Web.HttpContext.Current.Response.Write("<script>alert('" + msg + "')</script>");
        }
    }
}
在使用时
using YClass
YAlert a1 = new YAlert();
a1.Alert("hello world");
