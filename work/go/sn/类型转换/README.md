package main

import (
    "fmt"
)

func main() {
    str := "hello"
    arr := []byte(str)
    fmt.Println(arr)
    str2 = string(arr[:])
    fmt.Println(str2)
}