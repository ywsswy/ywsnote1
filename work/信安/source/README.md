import numpy
key_word = str.upper(input('请输入秘钥'))
key_set = list(set(key_word))
key_set.sort(key=key_word.index)
for i in range(26):
    ch = chr(i+0x41)
    if ch not in key_set:
        key_set.append(ch)
key_set.remove('J')
table = numpy.array(key_set)
table = numpy.reshape(table,(5,5))
table = table.T#by column
print(table)
M = list(str.upper(input('请输入明文')))
try:
    M[M.index('J')] = 'I'
except:
    pass
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
    elif left == 1:
        M.append('X')
    elif M[i] == M[i+1]:
        M.insert(i+1,'X')
    else:
        pass
    ra,ca = numpy.where(table == M[i])
    ra = ra[0]
    ca = ca[0]
    rb,cb = numpy.where(table == M[i+1])
    rb = rb[0]
    cb = cb[0]
    print(table[ra,ca],table[rb,cb])
    ytype = yjudge(table,ra,ca,rb,cb)
    print(ytype)
    if ytype == 1:
        C = C+table[ra,(ca+1)%5]
        C = C+table[rb,(cb+1)%5]
    elif ytype == 2:
        C = C+table[(ra+1)%5,ca]
        C = C+table[(rb+1)%5,cb]
    else:#the same line
        C = C+table[ra,cb]
        C = C+table[rb,ca]
    i += 2
print(C)
