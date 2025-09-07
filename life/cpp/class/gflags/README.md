参数fsync_peer
在代码中就是使用FLAGS_fsync_peer，

cc文件中：
DEFINE_string(fsync_peer, "default value", "desc");  // DEFINE_int32/DEFINE_bool

在另外一个cc文件中如果要使用，则需要头文件中：
DECLARE_string(fsync_peer)  // 相当于extern FLAGS_fsync_peer;

可以动态修改
FLAGS_fsync_peer = "XXX";

#include <gflags/gflags.h>

int main(int argc, char** argv) {
  google::ParseCommandLineFlags(&argc, &argv, true);  // 执行完这一行后，所有-开始的参数都会被取走（argc会变小），而没有-的参数会保留，例如./a.out hello --age=10 world，会被处理剩下./a.out hello world
  // ...

执行二进制时有五种方式
1）
--fsync_peer <value>
2）
--fsync_peer=<value>
3）
-fsync_peer <value>
4）
-fsync_peer=<value>
5）
--flagfile=/usr/local/conf.flags 这个文件中每一行写形如（-fsync_peer=<value>或者--fsync_peer=<value>，不能用空格）的参数

- 如果有未定义的参数，代码会报错&exit(1)；但是flagfile文件里面的内容允许未定义；
- 命令行参数和flagfile文件可以同时存在，以最后出现的为准；
- bool ture的写法：

```
-flag
--flag
-flag=true
--flag=true
-flag true  # （这是多余的写法，实际true四个字符并不起作用，不会被处理，应该避免这种写法，在某些解析器中，例如golang github.com/urfave/cli，甚至会【错误】影响后面的参数）
--flag true  # （这是多余的写法，实际true四个字符并不起作用，不会被处理，应该避免这种写法，在某些解析器中，例如golang github.com/urfave/cli，甚至会【错误】影响后面的参数）
```

- bool false的写法：

```
<不写>（默认未false时）
-flag=false
--flag=false
-flag false  # （这是错误的写法，false不会起作用，甚至flag被认为是true）
--flag false  # （这是错误的写法，false不会起作用，甚至flag被认为是true）
```