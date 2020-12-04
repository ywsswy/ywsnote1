/////////////////////////////////////
解决方案一：
把Response、Request、Session写全：
    System.Web.HttpContext.Current.Response
    System.Web.HttpContext.Current.Request//我是这个
    System.Web.HttpContext.Current.Session
 
解决方案二：
Application_Start代码段中不使用System.Web.HttpContext.Current
比如
    System.Web.HttpContext.Current.Request.MapPath
可改用
    System.Web.Hosting.HostingEnvironment.MapPath
 
解决方案三(不推荐)：
    应用程序池，托管管道模式，设为经典
