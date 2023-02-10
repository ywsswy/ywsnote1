# 生成器
# 只要函数中有yield关键字，这个函数的调用就不是执行，而是直接返回一个生成器
# https://zhuanlan.zhihu.com/p/268605982?utm_source=ZHShareTargetIDMore

def myGenerator():
    print('first')
    yield None #每次yield就返回，并且next()时是从上次返回的位置继续执行
    print('second')
    yield None #结尾返回后如果再next()会抛异常


class MyGlobalData(object):
    my_generator = myGenerator() #实例化generator，此时并没有进入内部，这里就跟普通函数不同，如果是普通函数这里就会打印first了
    

def main():
    while True:
        print('start')
        print(next(MyGlobalData.my_generator)) #python3用法，进入内部


if __name__ == '__main__':
    main()
