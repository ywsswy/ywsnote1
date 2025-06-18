dorker的操作应该都是root用户（普通用户：https://www.cnblogs.com/franson-2016/p/6412971.html）
非常好玩的是，两个人同时attach一个docker，那么两个人的终端就同步了，一个人操作另一个人能看

镜像仓库服务（docker registry）有点类似github

# 拉取镜像
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
docker pull 10.22.23.45:8080/mysql/mysql-server:latest

# 列出所有镜像（任何人拉相同的镜像的image ID都会是相同的）
docker images

# 删除镜像
docker rmi <image ID>

Docker本质上是一个运行在Linux操作系统上的应用，而Linux操作系统分为内核和用户空间，无论是CentOS还是Ubuntu，都是在启动内核之后，通过挂载Root文件系统来提供用户空间的，而Docker镜像就是一个Root文件系统。
# 运行镜像image（创建容器container）
docker run -m 10240M -it -v <本地目录>:<容器目录> -v <本地目录2>:<容器目录2> --network=host --name <NAMES> --security-opt seccomp=unconfined <IMAGE ID> /bin/bash -l # 安全参数是为了便于单步调试，后面/bin/bash -l 是指定shell并且用登录的方式，否则/etc/profile文件不会被加载；--privileged参数好像不好用。。是为了给高权限，因为docker里面执行不了systemctl命令
示例建议：（需要注意的是如果root里面建立的超出去的软链接，也需要-v映射；-m是对物理内存做限制，超出会触发oom机制，如果有些镜像对根目录有特殊要求，例如有特殊的.bash_profile那么，最好不要-v /root/:/root/）
docker run -it -v /root/:/root/ --network=host --name test --security-opt seccomp=unconfined xxx /bin/bash -l

-v /root/:/root/ 可能会影响LIBRARY_PATH环境变量？

如果加了-d 表示后台run，启动后不会占用当前终端，你可以继续执行其他命令 

# 查看各个docker的状态，STATUS有 Exited Up等
docker ps -a

# 删除容器
docker rm -f <CONTAINER ID>

# 重命名容器
docker rename <old_name> <new_name>

# 查看容器详细信息
docker inspect <CONTAINER ID>

# 从 Exited状态启动起来
docker start <CONTAINER ID>

# 进入（“接管”）Up状态的docker内部
docker attach <CONTAINER ID>

# 还有一种“不接管”的方法，新建一个终端，不担心exit时影响容器运行
docker exec -it <CONTAINER ID> /bin/bash -l

# 在docker内部退出来，且变为Exited状态
exit即可

# 在docker内部退出来，但是不结束docker
ctrl + p + q

# docker info（Docker Root Dir）查看docker pull的存放路径，默认是/var/lib/docker目录，可以使用软链接把额外挂载的磁盘文件系统的docker目录挂载到/var/lib下来实现更换Docker Root Dir的效果

# 在host上查看某容器内部的进程，注意这里的pid跟容器内部看到的pid不一样，但是实际是对应的；
docker top <container_name> <ps_options> 