函数使用方式
$(<function_name> [<args1>,<arg2>[,...]]

eg: SRC=$(notdif ..) #去除所有目录信息，SRC只有文件名
       =$(wildcard *..cpp) #得到所有cpp结尾的文件