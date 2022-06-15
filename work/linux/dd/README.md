将一个文件中的二进制数据提取出来
dd bs=1 skip=<byte_count> count=<byte_count> if=<input_file> of=<output_file>

没有if参数时是从标准输入读
echo "hello" > file 等价于 echo "hello" |dd of=file