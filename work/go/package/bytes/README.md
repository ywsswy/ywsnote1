```
package main

import (
  "bytes"
  "fmt"
)

func main() {
  slice1 := []byte{1, 2, 3}
  slice4 := []byte{0x1, 0x2}

  if bytes.Equal(slice1, slice4) {
    fmt.Println("slice1 and slice4 are equal")
  } else {
    fmt.Println("slice1 and slice4 are not equal")
  }
}
```