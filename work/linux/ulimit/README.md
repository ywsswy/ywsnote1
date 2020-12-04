程序segment fault(core dump)就需要gdb .bin core_file，进入bt调用栈查看最后运行到哪句话

但是要保证core文件的没有限制
就使用ulimit -c unlimited
