## go install proxy/path   //path下会有go文件
go: proxy/path1@tag?-version requires
  proxy/path2@tag?-version requires
  proxy/path3@tag?-version invalid version: xxxx:exit status 128:
  fatal ...

这是因为该依赖包找不到了，可能远程仓库path3已经被删了