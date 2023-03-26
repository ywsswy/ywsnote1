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
    foo.ptr = C.LIB_NewFoo(C.int(value))  # 【1】【6】LIB_NewFoo是在library-bridge.h中声明的C风格函数，其入参是int，其返回值在c中是void *，对应golang中的unsafe.Pointer 
    return foo
}

func (foo Foo) value() int {
    return int(C.LIB_FooValue(foo.ptr))  # 【2】
}

func (foo Foo) Free() { ... }

func main() {
    foo := NewFoo(42)
    defer foo.Free() // 让c++去析构对应的内存
}
```

bazel 不会生成.a，不过生成的.so也是能用的

【1】传递int给c++
【2】从c++接收int
【3】传递go字符串给c++，在go中创建指针，go负责(defer)释放指针
```
s := "xxx"
cs := C.CString(s)  // cs的类型是*_Ctype_char，等价于c中的char*
defer C.free(unsafe.Pointer(cs))  // 在go中创建的指针由go负责释放，普通类型无需释放，C.free必须接收unsafe.Pointer类型的
C.Func(cs, C.int(len(s)))  // void Func(const char* cs, const int length); // C.int(len(s))的类型是_Ctype_int，等价于c中的int
```
【4】从c++接收字符串（转换成go字符串），应该在c++中释放，所以可以留一个释放的接口给go调用
```
【4-1】接收char*
var buf string := C.GoStringN(cs, length)  // cs的类型是*_Ctype_char，即c++中的char*，length的类型是_Ctype_int
【4-2】接收std::string，因为接口都是extern "C"的，所以如果用c++的std::string，只能以指针（void *）的形式来出现，所以这个就是接收普通指针的用法
cErrStrPtr := C.NewString() // c++实现void *NewString() { return new std::string(); }
defer C.DeleteString(cErrStrPtr)  // c++实现void DeleteString(void *p) { std::string *ps = reinterpret_cast<std::string *>(p); delete ps; }
// use cErrStrPtr
```
【5】传递指针给c++
- unsafe.Pointer传递给void*
- *_Ctype_char传递给char*
【6】从c++接收指针，见【4-2】