## map
// c++中的std::map<std::string, std::string>在golang中：
var c map[string]string  // 等价于var c map[string]string = nil；虽然fmt输出是map[]，但是其值实际是等于nil的，不能直接使用（len(c)能用），必须再用make初始化
d := map[string]string  // 隐式类型推导语法错误
e := map[string]string{}  // 语法正确，等价于var e map[string]string = map[string]string{}

## 查找
value, ok := bb["x"]  // ok是bool类型，如果是true表示找到，然后value是找到的元素的拷贝（根据元素类型来决定是引用拷贝还是值拷贝），找不到也不会像c++那样新增一个空元素；
for key, value := range bb {  // 类似，key和value是元素的拷贝
}

显然如果元素类型是struct，那么修改value并不能达到修改原map的目的，所以要么定义成*struct，要么把value修改完后，重新赋值给bb["x"]，进而间接实现修改map的目的；

## 两个map的比较
因为map无序，所以要么逐个key去另一个map里find
要么变成有序的map来序列化

https://raw.githubusercontent.com/simagix/gox/master/ordered_map.go


## 删除元素（不可以在遍历的时候删）
delete(a, "key")

## set
golang没有内置set
但是可以自己写type Set map[string]struct{}
其中struct{}就是0占用空间的，相当于不关心值，只关心key