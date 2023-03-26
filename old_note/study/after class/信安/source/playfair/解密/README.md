import numpy
key_word = str.upper(input('请输入秘钥：'))
key_set = set()
for i in key_word:
    if i.isalpha():
        key_set.add(i)
key_set = list(key_set)
key_set.sort(key=key_word.index)
for i in range(26):
    ch = chr(i+0x41)
    if ch not in key_set:
        key_set.append(ch)
key_set.remove('J')
table = numpy.array(key_set)
table = numpy.reshape(table,(5,5))
model = int(eval(input('请输入填充模式（0：按列，1：按行）：')))
if model == 0:
    table = table.T#by column
else:
    pass
print(r'''密钥矩阵（i/j同位）：
{}'''.format(table))
mes = str.upper(input('请输入密文：'))
M = []
for i in mes:
    if i.isalpha():
        M.append(i)
        
        
def yjudge(table,ra,ca,rb,cb):
    if ra == rb:
        return 1
    elif ca == cb:
        return 2
    else:
        return 3 
        
i = 0
C = ''
while True:
    len_m = len(M)
    left = len_m - i
    if left == 0:
        break
    ra,ca = numpy.where(table == M[i])
    ra = ra[0]
    ca = ca[0]
    rb,cb = numpy.where(table == M[i+1])
    rb = rb[0]
    cb = cb[0]
    #print(table[ra,ca],table[rb,cb])
    ytype = yjudge(table,ra,ca,rb,cb)
    #print(ytype)
    if ytype == 1:#同行在右侧
        C = C+table[ra,(ca+4)%5]
        C = C+table[rb,(cb+4)%5]
    elif ytype == 2:#同列在下侧
        C = C+table[(ra+4)%5,ca]
        C = C+table[(rb+4)%5,cb]
    else:#不同行不同列，取同行的那个角
        C = C+table[ra,cb]
        C = C+table[rb,ca]
    i += 2
print(r'''明文：{}'''.format(C))
