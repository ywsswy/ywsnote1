// 单测 + mock https://github.com/agiledragon/gomonkey
// 运行某个test函数，-gcflags=all=-l可以避免编译器优化导致mock不成功
go test -gcflags=all=-l -run <Test_function_name> -v ./<sub_folder>/
- 文件名是_test.go结尾的，就不会导出（外部就不会访问到）
- vscode可以直接可视化单测

func Test_some(t *testing.T) {  // 要求单元测试函数必须以Test开头，并且函数参数是t *testing.T
	p := gomonkey.ApplyFunc(pac.fun, func() string {  // 把所有调用pac包中的fun函数都mock掉，固定返回"hello"
		return "hello"
	})
	defer p.Reset()
	t.Run("自定义测试名", func(t *testing.T) {
		if xxx {
			t.Errorf("erryyy")  // 只要执行到t.Errorf的就会被认为是错误
		}
	}
}

func fun1() {
  var bbb BBB = {}
  x,y := bbb.Delete()  // 远程rpc调用
  xxx
}

func Test_trsDeleteMsg(t *testing.T) {
	outputs := []gomonkey.OutputCell{
		{Values: gomonkey.Params{1, 2}, Times: 2}, // 前两次调用 x,y := 1,2
		{Values: gomonkey.Params{11, 22}, Times: 1}, // 第三次调用 x,y := 11,22
	}
	var p = gomonkey.ApplyMethodSeq(reflect.TypeOf(bbb), "Delete", outputs)  // 把所有BBB类型变量的Delete函数都mock掉
	defer p.Reset()
	t.Run("自定义测试名", func(t *testing.T) {
		fun1()  
		fun1()
		fun1()
	})
}