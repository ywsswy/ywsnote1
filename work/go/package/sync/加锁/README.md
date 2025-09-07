package main

import (
    "fmt"
    "sync"
    "time"
)

func main() {
    var mu sync.Mutex
    var counter int

    for i := 0; i < 10; i++ {
        go func() {
            mu.Lock()
            counter++
            mu.Unlock()
        }()
    }
    time.Sleep(time.Second)
    fmt.Println("Final Counter:", counter)
}