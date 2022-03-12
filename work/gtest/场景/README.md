目录：

示例A的适用场景：
1）类CA的FA方法中有调用另一个单例类CB的非虚方法FB，（单例类的实现是static T *GetInstance），希望在调用FA方法时，把FB的实现mock掉；