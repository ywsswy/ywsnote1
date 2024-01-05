package main

import (
"sync"
"fmt"
)

var (
  initOnce sync.Once
)

func initAAA() {
  initOnce.Do(func() {  // 只有第一次才会执行
    fmt.Printf("hello\n")
  })
}

func F1() {
  initAAA()
}

func main() {
  F1()
  F1()
  F1()
  F1()
  F1()
}