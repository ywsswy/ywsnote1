//类似其他语言的map，键值对，无序
dict的key必须是不可变对象
d1 = dict()#或者 d1 = {} 或者 d1 = {'yws':'awesome'}
【访问/修改
d1['yws'] = 'awesome'#
【方法
判断是否存在某键
'Thomas' in d #返回bool
【遍历
for key, value in a.items():
       print(key+':'+value)

【如果希望有序sorted(a.items())

两个dict可以直接比较是否相等 == 