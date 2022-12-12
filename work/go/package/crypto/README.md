package main
 import (
   "fmt"
   "crypto/md5"
 )
 func main() {
   re := md5.Sum([]byte("123456"))
   fmt.Printf("%x\n", re)  # e10adc3949ba59abbe56e057f20f883e
}