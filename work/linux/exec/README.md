这个命令有点危险
source执行完还在当前shell，但是exec执行完就退出当前shell了
因为exec的新进程PID没变


## 关于重定向的理解：
```
有几个（文件）描述符（软链接）/句柄
012开始时都是指向屏幕的，即软链接/proc/self/fd/[0/1/2]指向相同，即标准输入、标准输出、标准错误(键盘敲的实时回显）都能在屏幕看得到；
程序的输出会往这几个描述符输出（实际是往/etc/[stdout/stderr]输出，但是/etc/[stdout/stderr]又指向了/proc/self/fd/[1/2]）；
如果想重定向，则可以<cmd> 1><file1> 2><file2>   # 1>的1可以省略不写；因为cmd是新起了一个进程，所以重定向是新进程内的描述符指向，即只影响该程序的几种输出，并不会导致调用程序的终端出问题
为什么<cmd> 1><file1> 2>&1 的效果跟 <cmd> 2>&1 1><file1>的效果不一样呢；（往往期望的效果是前者，前者有一个简单的写法 &><file1>）
因为前者是ln -sf /proc/self/fd/1 <file>，然后ln -sf /proc/self/fd/2 /proc/self/fd/1，后一条语句相当于是ln -sf /proc/self/fd/2 <file>就相当于两个都指向一个文件了，
而后者是ln -sf /proc/self/fd/2 /proc/self/fd/1，然后ln -sf /proc/self/fd/1 <file>，就相当于描述符2其实原地没动；
如果不是要重新起一个进程改程序里的重定向，而是只想改当前终端的重定向，就需要借助exec命令
exec 2>&1 就是把当前终端的标准错误输出到标准输出，这句话写在脚本里，就可以保证脚本的结果全是标准输出（之所以这条命令执行完脚本没有像执行“exec echo '123'“那样结束，是因为exec没有检测到任何命令，也就没有新建进程，跟没有进程退出，那么重定向就也是在本进程内进行的）
参考：https://www.junmajinlong.com/shell/fd_duplicate/
```