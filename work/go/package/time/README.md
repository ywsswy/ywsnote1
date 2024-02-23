time.Sleep(1000 * 1000 * 1000)  //ns级别

time.After(1000 * time.Millisecond)
生成一个只可receive的chan，并且要等1s之后才能receive到(在这之前<-会阻塞)

timestamp := time.Now().Unix()  // s级别时间戳