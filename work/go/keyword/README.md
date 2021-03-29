nil 其实不是关键字,代表了pointer, channel, func, interface, map 或者 slice 的zero value
https://zhuanlan.zhihu.com/p/151140497
  var b *int
  fmt.Printf("%v\n", b == nil) //true