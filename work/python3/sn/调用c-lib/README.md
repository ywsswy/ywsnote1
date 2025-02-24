## 比golang简单，不需要.h以及链接操作，直接load so文件就行
```
import ctypes

library = ctypes.cdll.LoadLibrary("libfilter_vec.so")

# 写明libraray中接口函数的返回值类型和输入参数类型，空参数/void返回值可以不写restype/argtype
# c类型和ctypes的对应关系（https://blog.csdn.net/sinat_22510827/article/details/123435081），其中若要声明指针：ctypes.POINTER(xxx)
library.NewA.restype = ctypes.POINTER(ctypes.c_void_p)
library.ShowA.argtype = ctypes.POINTER(ctypes.c_void_p), ctypes.c_int  # 多个参数的写法
library.ShowA.restype = ctypes.POINTER(ctypes.c_void_p)
library.ReleaseA.argtype = ctypes.POINTER(ctypes.c_void_p)
library.ReleaseA.restype = ctypes.c_int  # 这是32位的有符号int

c = library.NewA()
d = library.ShowA(a, b)
e = library.ReleaseA(f)
```