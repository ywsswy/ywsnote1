Ubuntu系统 sh默认连接的是dash，这会造成某些命令找不到例如pushd

执行sudo dpkg-reconfigure dash 命令，将dash设置为No