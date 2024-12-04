```
ch := make(chan int)  // channel可以指定有缓存区容量，默认不写是0，即c := make(chan int, 0)
ch <- x    // Send x to channel ch.
<-ch       // Just receive from ch.
y := <-ch  // Receive from ch, and assign value to y.

当缓冲区满的时候，发送会阻塞（不执行后续语句）
当缓冲区空的时候，接受会阻塞
当二者同时阻塞（仅当缓存区容量为0时才有可能）时，可不经过缓冲区直接发送给接收

func fibonacci(n int, c chan int) {  // 定义函数参数时也可以定义成c chan<- int，因为这个函数只负责send；如果只负责receive的话，也可以定义成c <-chan int
	x, y := 0, 1
	for i := 0; i < n; i++ {
		c <- x
		x, y = y, x+y
	}
	close(c)  // 发送者可以close，表示再也不会发送了，所有的接收者接下来都不会阻塞了，都能取到默认值
}

func main() {
	c := make(chan int)
	go fibonacci(10, c)
	for i := range c {  // 接收者可以用for循环逐个接收，直到close为止
		fmt.Println(i)
	}
}

select语句的每个case必须是操作channel的；
select语句没有循环的能力，只有阻塞的能力：等到任意（不是按顺序）一个不阻塞的case，然后执行它并结束select语句；

select {
case i := <-c:
    // use i  // 可以用break跳出select、for、switch语句
case <-time.After(1000 * time.Millisecond):
    // 详见time，要一秒之后才能从这个管道读出数据，所以可以用于超时判断
default:  // 可以写default语句也可以不写，写了的话，当所有case都阻塞时则执行default语句然后会结束select语句；
    // xxxx
}
```
