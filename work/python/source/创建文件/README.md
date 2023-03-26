ybfile = open('./ymodules/ym1.py','w')
ybfile.write(r"""\
#Filename:ym1.py
gx = 10
def show():
    '''show the global gx'''
    global gx
    print(r'''{}'''.format(gx))
def hello():
    '''show the welcome information'''
    print(r'''hello world''')
""")
ybfile.close()
del ybfile
//覆盖
        with open(resFileName, 'w') as f:
            f.write(''.join(resList))
'r'
a=f.read()
