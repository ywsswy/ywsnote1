# mapValues
对map中的每个value进行处理

```
var wtf3 = Map(1 -> 2, 2 -> 4)
// 这两种写法等价
wtf3.mapValues(_ * 3)
wtf3.map((x:(Int, Int)) => (x._1, x._2 * 3))
```