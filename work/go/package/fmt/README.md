## 错误处理&输出
有error要一层层上报的话，最好return fmt.Errorf("msg|%w", err)

最上层打印的时候可以用fmt.Printf("%v\n", err)等价于fmt.Printf("%v\n", err.Error())

## 打印变量类型
fmt.Printf("%T\n", jsonRes)，等价于fmt.Printf("%v\n", reflect.TypeOf(x))

## 序列化：对于proto生成的struct，【序列化】成[]byte，（序列化不同于格式化输出，而是为了网络传输）
requestBody, err := proto.Marshal(qrwRequest)

## 格式化输出：对于struct / proto生成的struct,【格式化输出】成json格式的[]byte
```
j, err := json.Marshal(a)
fmt.Printf("%s\n", string(j)) // 其中json的字段就算是默认值也会输出，如果希望默认值不显示，在定义struct a时，应该加tag，设置json中的omitempty，例如  name string `json:"name,omitempty"` 详见struct中的tag
```
//暂定要美化（string struct error）就Printf("%v\n    
//要完整信息就Printf("%#v\n


## 多行字符串拼接
fmt.Sprintf(`%f`, 4.2)