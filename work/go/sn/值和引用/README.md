- 值类型包括：int、float、bool、string、结构体
- 引用类型包括：slice、map、channel、interface、指针
- 当将一个值类型的对象传递给函数时，实际上是将该对象的值复制一份，修改是改副本，而不是改原始对象，如下所示：
```
func f1(a string) {
	a = "666"
}
func f2(a *string) {
	a = "666"
}
func main() {
	h := "777"
	f1(h)  // h还是777
	f2(h)  // h是666
}
```

- 当将一个引用类型（例如map）的对象传递给函数时，虽然也是值拷贝（golang中不存在所谓的引用拷贝），但不是复制里面的key,value给新的map，而是复制了一些元信息，所以修改可以达到修改原始对象的效果；<font color=red>但是要看是哪一种修改类型，如果不是修改内容，而是修改指向新引用（例如 = &T{}）是达不到修改原始对象的效果的，</font>如下例子中map和slice的效果：
```
func f5(a map[string]string, b []string) {
	a["new_key"] = "new_value"  // 修改内容能达到改原始对象的效果
        a["new_key"] = nil  // 修改内容能达到改原始对象的效果
        a = nil  // 修改本身这就是新建的map了，跟原对象无关了，达不到改原始对象的效果
        b[0] = "HELLO"  // 修改内容能达到改原始对象的效果
        b = append(b, "world")  // 修改本身，达不到改原始对象的效果
}
func main() {
        a := map[string]string{"key":"value"}
        b := []string{"hello"}
        f5(a, b)
        fmt.Printf("%v%v\n", a, b)  // map[key:value new_key:new_value][HELLO]
}
```

- 如果结构体中有个成员是指针（外层结构体本身是值类型，但是其成员可以有引用类型），那么做函数参数传递时其普通成员修改是改副本，但是<font color=red>这个指针成员指向的内存可以改可以达到修改原始对象的效果，但是不能修改这个指针的值（指向的地址）</font>，
```
type MyS struct {
	B *int
}
func f3(a MyS) {
	*(a.B) = 10
}
func f4(a MyS) {
	t := 20
	a.B = &t
}
func main() {
	b := 0
	a := MyS{B: &b}
	f3(a)  // *(a.b) == 10
	f4(a)  // *(a.b) 仍然是10，因为指针的值没有被修改
}
```

- 修改指针“本身”不会影响其他指针的指向
```
package main

func main() {
  a := new(int)
  *a = 1
  b := new(int)
  *b = 2
  b = a  // b跟a指向相同的数据,2没有人指向了
  a = nil  // a指向空，但是并不会影响b，b还是指向1；
}
```