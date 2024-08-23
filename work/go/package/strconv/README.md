## 数值和string之间的转换
- 转数值
i, err := strconv.Atoi("4.2") //if err == nil 说明转换报错
i, err = strconv.ParseInt("4.2", 10, 8)  // 进制, bit数（不是字节数），这个就是int8
i, err = strconv.ParseUint("4.2", 10, 16)  // 这个就是uint16
i, err = strconv.ParseFloat("4.2", 32)


- 转字符串
strconv.Itoa(3)  // string(3)并不对
fmt.Sprintf("%f", 4.2)


## []byte和string之间的转换
    str := "hello"
    arr := []byte(str)
    str2 = string(arr[:])
