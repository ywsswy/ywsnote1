# 接口





查看一个变量的类型，使用空接口
	var i interface{}
	i = strconv.Itoa(3)
	fmt.Printf("%T(%v)\n", i, i)