1）创建动态数组，并返回一个引用了它的切片：

a := make([]string, 2, 6)  // len(a)=2, cap(a)=6，第三个参数可省

a = append(a, "hello") //这是往数组尾部添加，a[0] = a[1] = "", a[2] = "hello"

2）创建map（并非map指针）
s := make(map[string]string)
或者写成 s := map[string]string {}
但如果写成 var s map[string]string 是有问题的，此时是nil（虽然fmt输出是`map[]`），直接使用会crash
