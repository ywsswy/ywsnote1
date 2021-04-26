var (
  funcCCTVMain = "cctv_main_caller" //该文件？中的全局
)

func fun() {
  funcCCTVMain = "hello1" //全局
  fmt.Printf("%s\n", funcCCTVMain) //全局hello1
  funcCCTVMain := "hello2" //因为用了:=，变成了局部变量 
  fmt.Printf("%s\n", funcCCTVMain) //局部hello2
}


func main() {
  fmt.Printf("%s\n", funcCCTVMain) //cctv_main_caller
  fun()
  fmt.Printf("%s\n", funcCCTVMain) //hello1