//
一组key的集合
【新建
要创建一个set，需要提供一个list作为输入集合
s = set([1, 2, 3])
a = {1,2,3}
【运算
 s1 = set([1, 2, 3])
>>> s2 = set([2, 3, 4])
>>> s1 & s2
{2, 3}
>>> s1 | s2
{1, 2, 3, 4}
【
str是不变对象，而list是可变对象
set的原理和dict一样，所以，同样不可以放入可变对象，因为无法判断两个可变对象是否相等

【查找

x in s