```
- Label在continue, break中是可选的, 但是在goto中是必须的
- Label在嵌套函数(闭包)是不可用的

func main() {
    fmt.Println(1)
    goto End
    fmt.Println(2)
End:
    fmt.Println(3)
}


func main() {
    j := 1
FirstLoop:
    for {
      switch j { 
      case 0:
          fmt.Println(0)
      case 1:
          fmt.Println(1)
          break FirstLoop  // 跳“出”（而不是跳到）FirstLoop指定的for/switch/select，然后执行fmt.Printf("return\n")
                           // 普通的break只能跳出一层，但是加了标签可以跳出多层
      }   
    }
    fmt.Printf("return\n")
}


func main() {
FirstLoop:
    for i := 0; i < 3; i++ {
        for j := 0; j < 3; j++ {
            fmt.Printf("i=%v, j=%v\n", i, j)
            continue FirstLoop  // 不是跳出，而是执行FirstLoop指定的那一层的continue，
                                // 输出i=0~2, j=0
        }
    }
}

```
