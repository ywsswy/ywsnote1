- objdump -D 进行反汇编
每条指令的展示一行最多8字节，超过的部分换行，例如下例是9字节的指令，用2行展示
```
4007df: 48 c7 45 e0 00 00 00  movq   $0x0,-0x20(%rbp)
4007e6: 00
```


- objdump -r 查看重定向信息，在relocatable文件中，所有的函数调用指令都是没有写地址的`e8 00 00 00 00`，只有在链接截断才会根据重定向信息把地址填充到e0后面的四个字节里面，所以要分析relocatable文件中的函数跳转指令，必须使用objdump -r来查看其实际跳转到哪个地址了，输出形如：
```
RELOCATION RECORDS FOR [.text]:
OFFSET           TYPE              VALUE 
0000000000000023 R_X86_64_PLT32    _ZN3AAA4ShowEi-0x0000000000000004
0000000000000031 R_X86_64_PLT32    _Z3Funv-0x0000000000000004
```
表示.text section中的0x23位置的四字节调用地址应该填写的AAA::Show(int)函数的地址