区别

- IsInitialized()方法，proto2会检查是否require字段都赋值了，而proto3始终返回true（因为全部都视为option字段）
如果是false时，DEBUG模式的代码会glog FATAL退出：message_lite.cc:442] CHECK failed: IsInitialized(): Can't serialize message of type
非DEBUG模式的代码没影响

- 如何判断一个源文件是通过pb2生成的还是通过pb3
看是否有_has_bits_，有就是pb2


