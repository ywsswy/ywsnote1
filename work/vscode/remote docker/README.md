https://code.visualstudio.com/remote/advancedcontainers/develop-remote-host
这种方法可以不用在本地安装docker，只在远程开发机上安装即可

安装remote-ssh 和 dev container插件
然后在remote explorer上就有两种选项：Remote/Container
使用方法是：
先用Remote的选项链接到服务器，然后再使用Container选项attach到容器上，

从小到大的几个维度：
editor是一个文件
folder是一个工作目录
docker
remote是一个远程机器
window是这个vscode软件

如果docker内目录打开错了，就应该file->close folder然后再open一个folder

vscode很有问题：
1）有时候链接不上容器 或者打开容器的情况下，terminal不一定是准的，有可能是错乱的（其他容器的terminal环境）：一次成功的解决过程是：
手动attach进去把vscode相关进程全杀掉（注意不是宿主机的，而是容器内的）；
更快速的命令是在host机器上：docker top <container_name> xfu |grep vscode |grep -v grep |awk '{print $2}' |xargs --replace kill -9 {}
另外考虑不要在remote explorer里面进行attach项目，而是打开new window再从recent跳转；
2）~同一个容器没法打开多个workspace，所以干脆在root根目录打开一个算了~，但是这样代码跳转要解析的文件就太多了
3）golang无法设置120个字符自动换行格式化，目前是考虑用golangci-lint + .golangci.yml解决