首先要游戏运行（数据都加载到内存了）
然后打开ce，选择游戏进程；
针对游戏现状，在搜索框输入你觉得内存中应该对应的数据是多少，例如游戏里50金币，就猜测按照4字节整数搜索，输入50，点击[First scan]
然后回到游戏做操作，使得游戏数值改变，例如金币变成60，然后回到CE，输入60，点击[Next scan]，
一直操作下去，直到最后将范围逐渐缩小锁定到n个内存中的地址，
因为可能是n个，所以要逐个试，到底哪个才是最核心的变量的地址；
试的方法是把可疑的地址添加到下面的编辑栏，然后双击value去修改，看是否真的会在游戏里生效，
此外还可以锁定某个变量的值，让他始终等于某值（勾选前面的active即可）

每次scan有很多种选项：
维度1: value type
维度2: scan type
1）exact value：确定值（等于）
2）increased value：比上次扫描变大的值
3）decreased value：减小的值
4）unchanged value


有时候锁定值也无法影响游戏，这说明游戏数据不是“直接”使用变量的值，而是经过代码计算得到个临时变量，这时候就要修改代码段了