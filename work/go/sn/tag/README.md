Golang中可以为结构体的字段添加tag，这类似于Java中为类的属性添加的注解

type Req struct {
	Size  int32  `json:"size"`
	Type  int32  `json:"type"`
}



就是说这个不是简单的注释，可以通过反射机制知道某个字段后面注解里面的值，
相关应用有gin框架接受http参数时的处理，
使用func (c *gin.Context) ShouldBindQuery(obj interface{})，obj是带有form的注解，就可以把tag中的form后面的参数解析到前面的struct字段中


json.Marshal可以把tag中的json的值当作序列化后json的key