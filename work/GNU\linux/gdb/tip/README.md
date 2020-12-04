std::shared_ptr<<type>> var


what var

p * <var>._M_ptr #看智能指针的值

p *(<type>*)<address>




x/<num>a <addr>     #//x/1a 0xbfffef0c //从0xbfffef0c开始显示1个4字节（32位机器）或者8字节（64位机器）数据，可以用来看堆栈数据
x/<num/f/u> <addr>  # https://blog.csdn.net/yasi_xi/article/details/7322278
(gdb) p buf_
$35 = 0x9188000 "\020"
(gdb) x/12x 0x9188000    # x(16)进制显示12个字长数据，注意每个字长都是可视化的（真的等于c++中*(int32_t*) <addr>），虽然0x7b649efa在真实内存/磁盘中是fa 9e 64 7b
0x9188000:      0x00000010      0x7b649efa      0x2bca7a68      0x0000001c
0x9188010:      0x000000ff      0x93f67f33      0x4a1ddd98      0x0000001c
0x9188020:      0x000000ff      0x99901710      0x4e98057f      0x0000001c

dump binary memory log.txt yy_ec yy_ec+256 以二进制的形式把内存打印到文件中

## gdb发现断点行数/文件不对
可能是因为有这个宏
#line 3 "lex.yy.c"#表示从这行开始算是lex.yy.c文件的第3行（如果在下一行打印__LINE__的话，则是4）