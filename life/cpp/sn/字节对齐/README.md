// case1 位域

struct s
{
  unsigned char a : 1; //a占第一个2进制位
  uint32_t b : 3; //b占第二三四共三个二进制位
};
unsigned char c[1028] = {255,2,3,4,5,6,7,8};
s *s1 = (s *)c;
(gdb) p &s1.a
$1 = (unsigned char *) 0x7fffffffd9c0 "\377\002\003\004\005\006\a\b"
(gdb) p &s1.b //地址最详细只能看到字节单位，不能更精确到位，所以a和b的地址相同
$2 = (uint32_t *) 0x7fffffffd9c0
(gdb) p s1.a
$3 = 1 '\001'
(gdb) p s1.b
$4 = 7


/*
字节对齐的原则是一个成员的起始地址能整除以字节对齐值
1. 类、结构体的对齐原则：使用成员当中最大的对齐字节来对齐。比如在Struct A中，有int,char，int a的对齐字节为4，比char大，所以A的对齐字节为4
2. 使用宏 #pragma pack(n)可以指定对齐值，#pragma pack ()恢复默认
3. 最终的对齐值 = min(对齐原则，#pragma指定的对齐值)
4. 如果多个相邻成员的字节数之和小于对齐字节数，则这多个变量放在同一对齐块内；（见case4）
*/
// case2 默认字节对齐（int最大，所以用4字节对齐，结构体的sizeof是8）

struct s
{   
    unsigned char a;
    int b;
};
(gdb) p &s1.a
$1 = (unsigned char *) 0x7fffffffd9c0 "\001\002\003\004\005\006\a\b"
(gdb) p &s1.b
$2 = (uint32_t *) 0x7fffffffd9c4


// case3 默认字节对齐（int是4，最终 = min(4,2) = 2，结构体的sizeof是6）
#pragma pack (2)
struct s
{
    unsigned char a;
    int b;
}
(gdb) p &s1.a
$1 = (unsigned char *) 0x7fffffffd8f0 "\377\002\003\004\005\006\a\b\t\n"
(gdb) p &s1.b
$2 = (int *) 0x7fffffffd8f2
(gdb) 


// case4 sizeof = 8

struct s {   
    unsigned char a;
    unsigned char b;
    int c;
};
