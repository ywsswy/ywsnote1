--flagfile=/usr/local/trpc/conf/rimos.conf 这是gflag自带的参数，文件内的参数都会被加载到FLAGS_*


参数fsync_peer
在代码中就是使用FLAGS_fsync_peer，

在另外一个cc文件中如果要使用，则需要
DECLARE_string(fsync_peer)  // 相当于extern FLAGS_fsync_peer;

可以动态修改
使用SetCommandLineOption("fsync_peer", "666")


执行二进制时就是
--fsync_peer <value>