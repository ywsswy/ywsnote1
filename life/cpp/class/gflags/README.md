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
  google::ParseCommandLineFlags(&argc, &argv, true);
  // ...

执行二进制时有三种方式
1）
--fsync_peer <value>
2）
-fsync_peer=<value>
3）
--flagfile=/usr/local/conf.flags 这个文件中每一行写形如（--fsync_peer <value>）的参数
