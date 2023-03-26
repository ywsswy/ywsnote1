class Myt(object):
    a = 1 #类变量，所有实例共享
    
    def __init__(self):
        self.a = 2 #实例变量
    def pp(self):
        self.a = 3 #实例变量
        a = 4 #函数局部变量
        Myt.a = 5 #类变量
