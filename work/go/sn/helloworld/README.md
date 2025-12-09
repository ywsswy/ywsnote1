## 环境搭建（建议直接使用镜像），也可以直接拉构建好的：wget https://go.dev/dl/go1.21.1.linux-amd64.tar.gz，解压配置go/bin到PATH，然后配置go env -w GOPROXY/GOPRIVATE/GOSUMDB

判断自己的环境是否配置好，可以使用go env来检查，其输出中的几个信息是：
GOROOT 就是go的安装目录，go和gofmt本体在里面
GOPATH go install命令的目的地，import包时的搜索路径，其内的pkg目录存放编译好的库文件, 主要是*.a文件也可以是源代码地址

## start
```
>go mod init helloworld  # 新建一个模块，自动生成一个go.mod文件，记载着依赖包以及版本信息，可能跟项目中的tag不完全一致有生成规则，例如如果项目tag是abc/v1.0.2，那go.mod文件可能只写v1.0.2，把前面省略了
>cat main.go  # 编辑程序
package main

import "fmt"

func main() {
  fmt.Printf("hello wolrd\n")
}
>go build  # 生成可执行文件，如果go.mod中有require的东西且本机还没有，会下载到$GOPATH/pkg/mod目录下
# -ldflags "-w -s" 加这个参数可以去除符号表调试信息等降低文件大小
>./helloworld 
hello wolrd
```

## 其他说明
go mod tidy 命令会自动分析程序的依赖项并进行下载、清理（会增删require的模块但不会修改已指定的模块版本）go.mod文件、更新生成go.sum；所以go.sum是可以不上传到仓库的，有go.mod就够了

go mod graph  # 查看依赖关系，每一行\<A> \<B>表示A里面require了B

go mod download 命令会合法化 go.mod 文件中的依赖，并下载相应的模块和版本，以及更新 go.sum 文件以包含这些依赖项的哈希值。（所以应用场景是开发时手动改go.mod加完依赖后再执行该命令）

go fmt # 格式化当前package，（go本身没有line length limit，所以需要限制行长度需要使用https://github.com/segmentio/golines 工具：golines -w -m 120 .）

go env -w GOPROXY=https://goproxy.io,direct # 设置当前用户的配置（持久化修改），具体配置存放文件是GOENV指定的文件

go run <src.go> #直接执行

## go 1.17 版本前后，这两个命令的功能有变化

go install 命令不会修改 go.mod 或 go.sum 文件。它只会编译并安装指定的包或可执行文件（感觉没有应用场景，直接go build就完事了）

go get xxx@master # 缺少依赖的时候可以这样下载，会自动修改go.mod（而go install不会），这命令中包含了go build和go install，下载是下到$GOPATH/pkg/mod里面
