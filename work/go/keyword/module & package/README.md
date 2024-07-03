## 注意事项
```
> tree .
go.mod  // 一个【模块】只有一个go.mod文件，也可以没有go.mod（模块名就是仓库路径）
go.sum  // go.mod是版本依赖的语义化描述（人为编写），go.sum则是实际所下载的代码版本信息例如commit id或具体的某个tag（人不易读）
d1/f1.go
p2/f2.go
d3/go.mod  // 子目录中也可以放go.mod，放了就表明这是又一个独立的模块，（独立意味着跟外面的模块之间互相访问需要通过require的方式-视为其他仓库-来访问）
d3/f3main.go

> cat go.mod
module aaa/bbb/ccc  // 表明【模块名/路径】是aaa/bbb/ccc
                    // 通常如果自己的模块如果是要提交到仓库可供别人依赖的话，模块命名规则应该是repository location
                    // 例如module github.com/xxx/yyy/zzz 表明这个模块内的所有代码会上传到（别人可以拉取自）github的xxx/yyy/zzz路径
require x.com/y/z v1.0.3  // 写法是require <依赖的模块名> <合法的远程tag后缀 或 合法化的本地tag>
                          // 合法化是指，如果依赖的远程仓库没打tag只用的分支，或者就算打了tag但是其可能不规范，所以go-mod会修改go.mod文件内容，修改成一个类似v0.0.0-20220222080849-2dff500f8093的“本地”tag；
                          // 一个合法的远程tag格式要求：[<模块(go.mod文件)相对仓库的路径>/]va.b.c，其中a\b\c是数字
                          // 例如仓库是x.com/y，模块(go.mod文件)位于x.com/y/z中，那么规范的tag就应该打成z/va.b.c，不存在go.mod时，就相当于模块跟仓库同级，打tag就不用带上相对路径了

replace x.com/y/z v1.0.3 => ./test/z  // 如果希望修改依赖库，但是临时代码还没有打tag，可以拉取到本地，然后replace临时依赖本地代码

> cat d1/f1.go
package p1  // 【包名】是p1，【包（导入）路径】是aaa/bbb/ccc/d1，因为d1目录没有go.mod，所以要往上层目录找自己属于哪个模块进而确定的包路径

> cat p2/f2.go
package p2  // 包名是p2，包路径是aaa/bbb/ccc/p2，注意这里包名和包路径的最后一个元素相同，但是d1/f1.go例子则不相同

> cat d3/f3main.go
package main  // 必须是在包名为main的所在目录下才能进行go build生成可执行文件（生成的文件名默认为模块名的最后一个元素，这个例子是ccc）
// import的语法是`import [<【包别名】>] <包路径>`
import wtf "aaa/bbb/ccc/d1"  // 包别名可以由调用方随意起，并不一定要求跟包名一致
import "aaa/bbb/ccc/p2"  // 当包名和包路径的最后一个元素相同时，可以选择省略包别名，直接使用包名作为包别名；包路径可能含小数点，但是包名只能下划线

func main() {
  wtf.fun1()
  p2.fun2()
}

```
- 同一个目录（不含子目录）内的文件中声明的包名必须一致
- "import math/rand" 这种导入的包属于系统包，是在$GOROOT/src目录下的；
- v2及以上版本管理：https://mileslin.github.io/2020/08/Golang/%E5%88%B0%E5%BA%95-go-get-%E7%9A%84%E7%89%88%E8%99%9F%E6%80%8E%E9%BA%BC%E9%81%8B%E4%BD%9C%E7%9A%84/