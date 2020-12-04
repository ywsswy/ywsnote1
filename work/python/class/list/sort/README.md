按字母排序（会改变原始数据的位置）
想不变用sorted，或者.sort(key = l1.index)
a = [5,1,1,3,3,4,3,1]
print(a.index)
b = list(set(a))
print(b)
b.sort(key=a.index)
print(b) #[5, 1, 3, 4]
