## Webkit
WebKit 是一个开源的浏览器引擎，与之相对应的引擎有Gecko（Mozilla Firefox 等使用），Trident（也称MSHTML，IE 使用）和EdgeHTML（也称Chakra，Edge和其他UWP浏览器使用）
Webkit源于KDE开源项目，兴盛于苹果公司的Safari项目由两部分组成，一部分是WebCore排版引擎，用以解析HTML语言和CSS框架；另一部分为JSCore JavaScript执行引擎，用以执行网页JS脚本。
Chrome只是继承了Webkit的WebCore部分，在JS引擎上使用了Google引以为豪的“V8”引擎，大大地提高了脚本执行速度，这也是为什么Chrome会如此快的重要原因

## webkit的资源分类主要分为两大类：主资源和派生资源
主资源，比如HTML页面，或者下载项
派生资源，比如HTML页面中内嵌的图片或者脚本、样式表链接
这两类资源的加载过程颇有不同，比如对资源加载失败的处理，主资源下载失败会有报错提示，而派生资源如图片下载失败，往往只显示一个占位符

## 关于缓存
https://www.zhoulujun.cn/html/theory/ComputerScienceTechnology/network/2018_0306_8078.html
https://blog.csdn.net/weixin_44369568/article/details/92721315

服务器返回请求时添加头可无视一切缓存'Cache-Control':'no-cache,no-store,max-age=0,must-revalidate'