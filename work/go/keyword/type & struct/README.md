type用于定义/重命名结构体/接口

## 结构体初始化
type Ytestma struct {
  A  *int32
  C  []int32
}
对于这种结构体初始化的时候可以一次性初始化完，也可以后续初始化
  a := int32(12)
  pp := &ytestp.Ytestma {
    A: &a,
    C: []int32 {
      666,
      233,
    },  
  }

## 结构体的tag，类似于Java中为类的属性添加的注解
type Req struct {
	Size  int32  `json:"size"`
	Type  int32  `json:"type,omitempty"`
}

这个不是简单的注释，可以通过反射机制知道某个字段后面注解里面的值，
json.Marshal可以把tag中的json的值当作序列化后json的key，omitempty表示默认空值是否输出
相关应用有gin框架接受http参数时的处理，
使用func (c *gin.Context) ShouldBindQuery(obj interface{})，obj是带有form的注解，就可以把tag中的form后面的参数解析到前面的struct字段中

type NewType OldType  // 重命名

## 结构体（类）的方法
```
func (this *Ytestma) Double() { //就是在普通函数的函数名前面加上(this *<class>)，注意必须是指针，不然是深拷贝
	this.A = this.A * 2
}

func main() {
        a := int32(12)
	v := Ytestma{A: &a}
	v.Double()  // (&v).Double() 也能调用成功，golang没有`->`，所以`.`既能操作指针又能操作非指针
}
```

## "继承"成员(变量+函数)
```
type (
	C2 struct {
		C1  // 这种把第一个成员只写类型不写名称的，相当于把C1中的成员“继承“过来，这样C2的对象既可以访问.B，又可以访问.A，还可以调用f0()
		B int
	}

	C1 struct {
		A int
	}

	CC2 struct {
		*CC1  // 类似C2中的”继承“，但是因为是指针，所以对CC2的拷贝修改A原对象中A也会发生改变，而C2中则不然
		B int
	}

	CC1 struct {
		A int
	}
)
func (this* C1) f0() {
	fmt.Printf("f0\n")
}

func f1(cli C2) {
	cli.A = 11
}

func f2(cli CC2) {
	cli.A = 33
}

func main() {
	fmt.Printf("%v\n", "hello")
	cli1 := C2{
		C1: C1{
			A: 1,
		},
	}
	cli2 := CC2{
		CC1: &CC1{
			A: 3,
		},
	}
	f1(cli1)
	f2(cli3)
	fmt.Printf("%v\n", cli1.A)  // 1,没有成功修改
	fmt.Printf("%v\n", cli2.A)  // 33,可以成功修改
}

```
## "继承"成员函数（非变量）
type Client interface {  // Client是接口（一组方法的生命），不能实现方法，不能定义成员变量
	Name() string
}

type DClient struct {
}

func NewDClient() Client {
	ptr := &DClient{}
	return ptr  // 【2】只要代码里存在隐式转换，就相当于把“继承“的资格兑现了，而不需要像c++那样显式的写public继承
}

func (ptr *DClient) Name() string {  // 【1】只要DClient实现了Client中要求的函数，那么就自动具备了“继承“的资格，称DClient为：实现了Client接口的类；
	return "DClient"
}

type EClient struct {
}

func NewEClient() *EClient {
	ptr := &EClient{}
	return ptr
}

func (ptr *EClient) Name() string {
	return "EClient"
}

a := []Client{}
a = append(a, NewDClient())
a = append(a, NewEClient())  // 【3】这里也有隐式转换，从*EClient转成Client