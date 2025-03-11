## go install proxy/path   //path下会有go文件
go: proxy/path1@tag?-version requires
  proxy/path2@tag?-version requires
  proxy/path3@tag?-version invalid version: xxxx:exit status 128:
  fatal ...

这是因为该依赖包找不到了，可能远程仓库path3已经被删了


## go build 时候显示
verifying xxxx: 410 Gone

这是因为可能这个是内网的东西，Go 不知道 xxxx 是内网模块，会去 sum.golang.org 验证下载的模块的哈希，显然外网肯定没有内网模块的哈希值
解决办法是export GOPRIVATE=<内网域名>


## 网络问题
如果无法下载，看go env配置如果是默认的GOPROXY='https://proxy.golang.org,direct'，则可能会有网络问题，
可以配置代理，参考.bashrc

## ambiguous import
A imports
B imports
C ambiguous import: found package C in multivle modules:
C v0.1
C v0.2
因为没有在当前项目指定C的版本，通过其他间接依赖会有冲突，例如X导入的是v0.1的C，而Y导入的是v0.2的C，解决方法是在当前项目指明C的版本；
先通过go mod graph |grep C来判断都是谁间接依赖了C，然后可以选择其中最新版本的C，或者直接go get C@latest