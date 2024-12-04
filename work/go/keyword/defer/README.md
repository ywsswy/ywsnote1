- 1）defer 是退出所在函数时执行，而不是退出作用域时执行，可以有多个defer，先出现的后执行类似堆栈
- 2）defer执行的顺序在return 表达式之后，在函数调用的赋值之前，例如：

```
var a = 42
var b = 0

func f1() int {
  defer func() {
    a = 43
    fmt.Printf("in defer, b: %d\n", b)  // 0，【2】说明defer是在f1()结果赋值给b之前执行的
  }()
  var p *int
  p = &a
  return *p
}

func main() {
  b = f1()  // 【3】对b进行赋值
  fmt.Printf("a: %d\n", a)  // 43
  fmt.Printf("b: %d\n", b)  // 42，【1】说明return的表达式（*p）是最先执行的，表达式结果会先保存到一个临时无名变量中
}
```

另一个例子：

```
var err1 error = fmt.Errorf("err1 old err")

func f2() error {
	defer func() {
		err1 = fmt.Errorf("err1 new err")
	}()
	return err1
}

func main() {
	err2 := f2()
	// 这里是把临时的无名拷贝赋值过来的
	fmt.Printf("err1:%s\n", err1.Error())  // err1:err1 new err，说明defer里成功修改了err1
	fmt.Printf("err2:%s\n", err2.Error())  // err2:err1 old err，说明【1】return的表达式结果会先保存到一个临时无名变量中，然后【2】执行defer，然后【3】把临时无名变量赋值给err2；
}
```
