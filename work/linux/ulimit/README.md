程序segment fault(core dump)就需要gdb .bin core_file，进入bt调用栈查看最后运行到哪句话

但是要保证core文件的没有限制
就使用ulimit -c unlimited  # 使用ulimit -c查看

设置core文件的名称&路径
echo "./core.%e.%p" >/proc/sys/kernel/core_pattern

ulimit -n # 查看文件句柄数上限配置
ulimit -n <nofile_count> # 临时设置文件句柄数上限