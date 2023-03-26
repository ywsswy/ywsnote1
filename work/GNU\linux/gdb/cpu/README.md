16个32位寄存器：
4个数据寄存器（EAX、EBX、ECX、EDX），6个段寄存器（ES、CS、SS、DS、FS、GS），1个标志寄存器（EFlags），
2个变址和指针寄存器（ESI和EDI），//存段内偏移量
2个指针寄存器（ESP和EBP），//存堆栈内偏移量
1个指令指针寄存器（EIP），

A.主调函数
push形参，call（push返回地址,goto），
B.被调函数开始
push ebp（保存主调函数状态）
move esp-》ebp（ebp在一个函数内部固定，能借此随时获取传递来的参数）
  //例如 0x8(%ebp)就能获取第一个函数形参
C.被调函数内部，不论怎么push pop改变的都是esp，不影响ebp
D.被调函数结束
leave（move ebp-》esp; pop ebp）//恢复被调函数开始前的状态
ret（pop返回地址,goto）


# 备注：
layout asm可以查看汇编