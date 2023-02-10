i是静态变量,你使用类名.i调用就可以了
static 修饰的变量和方法不要用new
去掉定义变量new对象直接类名
 System.out.println("st1.i= " + StaticTest.i);
