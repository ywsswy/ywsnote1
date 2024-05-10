【超链接】
<a href=http:www.51zxw.net>我要自学网</a>
HyperLink h1 = new HyperLink();
h1.NavigateUrl = "http://www.51zxw.net";
hl.Text="我要自学网";
PanleControl.Controls.Add(hl);
BulletedList - HyperLink模式
【Response.Redirect】
重定向
Response.Redirect("7-4_newslist.aspx");
服务器给浏览器一个重定向指令
浏览器显示目标地址
【Server.Transfer】
可能有第三方控件冲突
服务器直接重定向，但是只能本站内的url跳转
浏览器不显示目标地址，可用来隐藏地址
【PostBackUrl】
在具有IButtonControl接口的控件中具有的属性
add：
~/img/01.jpg
js
window.location.href='http://www.ddhbb.com';
            Response.Redirect("~/Check.aspx");
