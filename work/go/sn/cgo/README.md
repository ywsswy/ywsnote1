golang里调用c++
https://blog.csdn.net/qq_38329894/article/details/126350610

main.go如下，注意的是，import "C"前面必须紧紧跟着“注释状态”include和特定的预编译指令#cgo LDFLAGS: -L./ -l3 -lstdc++
```
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
    foo.ptr = C.LIB_NewFoo(C.int(value))  # LIB_NewFoo是在library-bridge.h中声明的C风格函数，其返回值在c中是void *，入参是int
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