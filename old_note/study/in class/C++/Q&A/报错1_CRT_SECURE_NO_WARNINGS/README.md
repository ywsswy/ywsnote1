warning C4996: 'strcpy': This function or variable may be unsafe. 
Consider using strcpy_s instead. To disable deprecation, 
use _CRT_SECURE_NO_WARNINGS. See online help for details.
1>          c:/program files/microsoft visual studio 
10.0/vc/include/string.h(105) : 参见“strcpy”的声明
在给出警告时，都会告诉你相应的安全函数，查看警告信息就可以获知，
在使用时也再查看一下MSDN详细了解。库函数改写例子：
mkdir改写为 _mkdir 
fopen”改写为 fopen_s 
stricmp改写为 stricmp_s
strcpy改写为strcpy_s
    解决方案：
1> 根据下面的warning提示：参见“fopen”的声明
        消息:“This function or variable may be unsafe. Consider using fopen_s instead. To disable deprecation, 
use _CRT_SECURE_NO_DEPRECATE. See online help for details.”
        所以可以将函数按warning提示的第二句，改为使用fopen_s函数即可：
        例如:FILE *pFile=fopen("1.txt", "w");
           改为：
           FILE* pFile;
           fopen_s(&pFile, "1.txt", "w"); 
2> 还是根据warning提示的地三句话:use _CRT_SECURE_NO_DEPRECATE
        项目|属性|配置属性|C/C++|命令行|附加选项,加入【/D "_CRT_SECURE_NO_DEPRECATE" 】(注：加入中括号中完整的内容)
3> 降低警告级别：项目|属性|配置属性|C/C++|常规,自己根据情况降低警告级别（此法不推荐）
    注意：高度重视警告：使用编译器的最高警告级别。应该要求构建是干净利落的（没有警告）。理解所有警告。
通过 修改代码而不是降低警告级别来排除警告。
