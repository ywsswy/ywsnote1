## 对于struct / proto生成的struct, 【格式化输出】成json格式的[]byte
j, err := json.Marshal(a)
fmt.Printf("%v\n", string(j)) // 其中json的字段就算是默认值也会输出，如果希望默认值不显示，在定义struct a时，应该加tag，设置json中的omitempty，例如  name string `json:"name,omitempty"` 

## 对于proto生成的struct，【序列化】成[]byte，（序列化不同于格式化输出，而是为了网络传输）
requestBody, err := proto.Marshal(qrwRequest)

## 格式化输出
type YwsData struct {
  name string
  num int32
}
//暂定要美化（string struct error）就Printf("%v\n    
//要完整信息就Printf("%#v\n

## 结构体初始化
type Ytestma struct {
  A                    *int32
  C                    []int32
}
对于这种结构体初始化的时候可以一次性初始化完，
也可以后续初始化，但是要小心是指针

  a := int32(12)
  pp := &ytestp.Ytestma {
    A: &a, 
    C: []int32 {
      666,
      233,
    },  
  }

