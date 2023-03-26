#生成器

def myGenerator():
    print('first')
    yield None #每次yield就返回，并且next()时是从上次返回的位置继续执行
    print('second')
    yield None #结尾返回后如果再next()会抛异常


class MyGlobalData(object):
    my_generator = myGenerator() #实例化generator，此时并没有进入内部
    

def main():
    while True:
        print('start')
        print(next(MyGlobalData.my_generator)) #python3用法，进入内部


if __name__ == '__main__':
    main()
