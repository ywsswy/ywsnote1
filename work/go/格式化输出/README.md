对于struct / proto生成的struct, 格式化输出成json格式的[]byte
j, err := json.Marshal(a)
logger.Debugf("a err[%v], json[%v]", err, string(j))

对于proto生成的struct，序列化成[]byte，这个不是为了格式化输出，而是为了网络传输
requestBody, err := proto.Marshal(qrwRequest)

type YwsData struct {
  name string
  num int32
}
//暂定要美化（string struct error）就Printf("%v    要完整信息就Printf("%#v
func main() {
  fmt.Println("hello")
  fmt.Printf("world\n")
  ywsData := &YwsData{
    name: "123",
  }
  fmt.Printf("%#v\n", *ywsData) // 详细的输出每个key+value
  fmt.Printf("%v\n", *ywsData)
  fmt.Printf("%s\n", *ywsData)
  fmt.Printf("%d\n", *ywsData) // 该输出的还是会输出，只不过类型不匹配的会出现%!d(解释输出)的效果
}

main.YwsData{name:"123", num:0}
{123 0}
{123 %!s(int32=0)}
{%!d(string=123) 0}




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

"github.com/pkg/errors"
有error要一层层上报的话，最好return fmt.Errorf("msg\n\t%w", err)