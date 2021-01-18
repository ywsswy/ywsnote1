## 对于struct / proto生成的struct, 格式化输出成json格式的[]byte
j, err := json.Marshal(a)
fmt.Printf("%v\n", string(j)) // 其中json的字段就算是默认值也会输出，如果希望默认值不显示，在定义struct a时，应该加tag，设置json中的omitempty，例如  name string `json:"name,omitempty"` 

## 对于proto生成的struct，序列化成[]byte，这个不是为了格式化输出，而是为了网络传输
requestBody, err := proto.Marshal(qrwRequest)

## 格式化输出
type YwsData struct {
  name string
  num int32
}
//暂定要美化（string struct error）就Printf("%v    要完整信息就Printf("%#v
  fmt.Printf("%#v\n", *ywsData) // 详细的输出每个key+value
  fmt.Printf("%v\n", *ywsData)


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

## 关于error
"github.com/pkg/errors"
有error要一层层上报的话，最好return fmt.Errorf("msg\n\t%w", err)