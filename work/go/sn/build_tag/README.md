旧语法（Go 1.16 及之前）
使用注释形式，称为 build tags：
// +build sometag

新语法（Go 1.17 引入）
使用 //go:build 指令形式，官方术语为 build constraints（构建约束）：
//go:build sometag
源文件第一行写上例，表示go build指定sometag参数时（go build -tags sometag）才编译此文件（除非go build后面明确指定该文件名），
可以用来控制某目录不被golist扫描（让IDE不报错）

新语法支持标准的布尔表达式，
例如下例表示go build“未”指定tag_a参数时才编译此文件
//go:build !tag_a
