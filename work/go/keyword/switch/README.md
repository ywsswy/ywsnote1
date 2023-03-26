var data map[string]interface{}
err = json.Unmarshal(req.Value, &data)
for key, value := range data {
  switch value.(type) {  // value的类型是interface{}，json
  case string:  // golang的switch默认是有break的，所以命中就结束，不会执行下一个case，如果想要执行，则需要使用fallthrough关键字
  case bool:
        break  // 显式写break可以提前退出该switch语句块
        fmt.Printf("x")
  case float64:
	// golang-json中整形和浮点数都会被解析成float64
  default: