因为如果是继承的类型，type不同，但其实无所谓

使用时直接用(list(name)) 或者 (str(name))限制强制转换
判断时用isinstance(<name>,<class>)

isinstance(2,float) #False

isinstance("a",(str,unicode)) #支持多种类型类型检查


