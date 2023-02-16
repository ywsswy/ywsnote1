dorker的操作应该都是root用户（普通用户：https://www.cnblogs.com/franson-2016/p/6412971.html）
非常好玩的是，两个人同时attach一个docker，那么两个人的终端就同步了，一个人操作另一个人能看

镜像仓库服务（docker registry）是一个类似github的东西

# 拉取镜像
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
docker pull 10.22.23.45:8080/mysql/mysql-server:latest

# 列出所有镜像
docker images

# 删除镜像
docker rmi <image ID>

Docker本质上是一个运行在Linux操作系统上的应用，而Linux操作系统分为内核和用户空间，无论是CentOS还是Ubuntu，都是在启动内核之后，通过挂载Root文件系统来提供用户空间的，而Docker镜像就是一个Root文件系统。
# 运行镜像image（创建容器container）
docker run -m 10240M -it -v <本地目录>:<容器目录> -v <本地目录2>:<容器目录2> --network=host --name <NAMES> --security-opt seccomp=unconfined <IMAGE ID> /bin/bash # 安全参数是为了便于单步调试，后面/bin/bash是指定shell；--privileged参数好像不好用。。是为了给高权限，因为docker里面执行不了systemctl命令
示例：（需要注意的是如果root里面建立的超出去的软链接，也需要-v映射；-m是对物理内存做限制，超出会出发oom机制，如果有些镜像对根目录有特殊要求，例如有特殊的.bash_profile那么，最好不要-v /root/:/root/）
docker run -m 10240M -it -v /root/:/root/ --network=host --name test --security-opt seccomp=unconfined xxx


# 查看各个docker的状态，STATUS有 Exited Up等
docker ps -a

# 删除容器
docker rm -f <CONTAINER ID>

# 重命名容器
docker rename <old_name> <new_name>

# 从 Exited状态启动起来
docker start <CONTAINER ID>

# 进入Up状态的docker内部
docker attach <CONTAINER ID>

# 在docker内部退出来，且变为Exited状态
exit即可

# 在docker内部退出来，但是不结束docker
ctrl + p + q