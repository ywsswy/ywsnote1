golang里调用c++

```
g++ -static -c library.cpp 
g++ -static -c library-bridge.cpp 
ar rcs lib3.a library.o library-bridge.o  # 注意 g++ 在 link 阶段 .o 或 .a 的顺序是非常重要的，某个 o 文件只能调用在它后面列出的 o 文件

# 到这里，如果是c++使用这个lib的话，只需要main.cpp中
#include "library-bridge.h"
然后，放好lib3.a和library-bridge.h两个文件
使用g++ main.cpp -L./ -l3即可

如果是golang使用的话，同理main.go如下，注意的是，import "C"前面必须紧紧跟着“注释状态”include和特定的预编译指令#cgo LDFLAGS: -L./ -l3 -lstdc++
package main


//#cgo LDFLAGS: -L./ -l3 -lstdc++
//#include "library-bridge.h"
import "C"

import (
  "fmt"
  "unsafe"
)

type Foo struct {
    ptr unsafe.Pointer
}

func NewFoo(value int) Foo {
    var foo Foo
    foo.ptr = C.LIB_NewFoo(C.int(value))
    return foo
}

func (foo Foo) value() int {
    return int(C.LIB_FooValue(foo.ptr))
}

func main() {
    foo := NewFoo(42)
    // defer foo.Free() // The Go analog to C++'s RAII
    fmt.Println("[go]", foo.value())
    fmt.Println("----------------------")
    //params := "gohello"
}
```

bazel 不会生成.a，不过生成的.so也是能用的