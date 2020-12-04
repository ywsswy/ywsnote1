* ##是一个连接符号，用于把参数连在一起
#define  FOO(arg)   my##arg
// FOO(abc) 相当于 myabc

* #是“字符串化”的意思。出现在宏定义中的#是把跟在后面的参数转换成一个字符串
#define STRCPY(dst, src)   strcpy(dst, #src)
// STRCPY(buff, abc) 相当于 strcpy(buff, "abc")

* 这里有个容易疏忽的
"hello " "world" 和 "hello world"这两个字符串是相等的