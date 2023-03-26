每个源文件（哪怕属于同一个包）都可以定义"多个"小写字母开头的init()函数，该函数会在main函数调用之前被自动调用；
顺序是：
import > const > var > init() > main()
注意import a就会执行a中的const > var > init()

编码时，按照const var type func顺序来书写