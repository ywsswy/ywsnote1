# defer



func f1() error {
  var err error
  defer func() { //执行时机是在return之后，这里面的err是外面err的拷贝，所以修改的话，不会影响到f1函数的返回值。要想改变除非是指针
    fmt.Printf("f1.fun err\n")
    fmt.Printf("here err:%v\n", err)
    err = fmt.Errorf("f1.fun err")   
    fmt.Printf("here2 err:%v\n", err)
  }()
  fmt.Printf("f1 err\n")
  err = fmt.Errorf("f1 err")
  return err
}