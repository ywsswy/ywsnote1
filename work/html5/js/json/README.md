var ob1 = JSON.parse(s1); //由JSON字符串转换为JSON对象


Number.MAX_SAFE_INTEGER; //json内部所有的数都是用double浮点数实现的，哪怕是整数也是，所以只要大于9007199254740991就不安全了，而不是最大能到int64