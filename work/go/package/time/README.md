time.Sleep(1000 * 1000 * 1000)  //ns级别

time.Sleep(1 * time.Second) // 参数是time.Duration类型

time.Duration(10) * time.Second  // 创建10s的time.Duration，必须显式指定单位

time.After(1000 * time.Millisecond)
生成一个只可receive的chan，并且要等1s之后才能receive到(在这之前<-会阻塞)

from := time.Now()  //time.Time类型
time.Since(from)  // 计算时间差 time.Duration类型

timestamp := time.Now().Unix()  // s级别时间戳
t := time.Unix(timestamp, 0)  // 转成time.Time类型
