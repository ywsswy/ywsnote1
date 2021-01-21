dorker的操作应该都是root用户

# 列出所有镜像
docker images


镜像仓库服务（docker registry）是一个类似github的东西


# 拉取镜像
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
docker pull 10.22.23.45:8080/mysql/mysql-server:latest

# 查看本机的docker的信息：docker images

Docker本质上是一个运行在Linux操作系统上的应用，而Linux操作系统分为内核和用户空间，无论是CentOS还是Ubuntu，都是在启动内核之后，通过挂载Root文件系统来提供用户空间的，而Docker镜像就是一个Root文件系统。
# 运行镜像image（创建容器container）
docker run -it -v <本地目录>:<容器目录> --name <NAMES> <IMAGE ID>
示例：
docker run -it -v /root/:/root/ --name smartbox xxxxxxxxxx 

# 查看各个docker的状态，STATUS有 Exited Up等
docker ps -a

# 删除容器
docker rm -f <CONTAINER ID>

# 从 Exited状态启动起来
docker start <CONTAINER ID>

# 进入Up状态的docker内部
docker attach <CONTAINER ID>

# 在docker内部退出来，且变为Exited状态
exit即可

# 在docker内部退出来，但是不结束docker
ctrl + p + q