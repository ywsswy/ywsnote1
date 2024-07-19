package main

import (
    "encoding/json"
    "fmt"
)

func main() {
    jsonStr := `{
        "a": "John",
        "b": "",
        "c": 30,
        "d": 30.9,
        "e": 1,
        "f": 0,
        "g": true,
        "h": false,
        "i": {
            "city": "New York",
            "country": "USA"
        },
        "j": []
    }`

    var data map[string]interface{}  // 不知道json的类型的时候用这个来解析，但是这个包优先把数值解析成float64，不利于整型解析，考虑特殊情况用jsoniter "github.com/json-iterator/go"替代
    err := json.Unmarshal([]byte(jsonStr), &data)  // 如果data是个map里面有数据，Unmarshal并不会清空原数据，而是会做merge冲突的部分才进行覆盖(只管一级字段)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }

    for key, value := range data {
        fmt.Println("Key:", key)
        fmt.Println("Value:", value)

        switch value.(type) {
        case string:
            fmt.Println("Type: string")
        case float64:
            fmt.Println("Type: float64")
        case bool:
            fmt.Println("Type: bool")
        default:
            fmt.Println("Type: not pod")
        }
        fmt.Println("\n")
    }
}