strip --strip-debug --strip-unneeded \<file_name>

可以去除掉debug信息和无用符号，减少文件大小；

如果strip后的程序出了core_dump，仍然可以使用strip前的文件进行gdb