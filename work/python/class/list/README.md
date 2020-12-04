有序，可变，元素可不同
【定义
liname = [1,2,3]
【访问/修改
li[0]
li[-1]
li[1][2]
列表list（list）[,]
元祖tuple（广义表）(,)
字典dict（map）{ : }
【切片】
li[0:-1] #不要最后一个元素[0,倒数第1)，例如去除行尾换行符

【方法
计算长度len(<list name>)
【自定义排序
import functools
def cmp(a, b): # 返回正数，排序放到后面
    if b < a:
        return -1
    if a < b:
        return 1
    return 0
a = [1, 2, 5, 4]
print(sorted(a, key=functools.cmp_to_key(cmp)))
