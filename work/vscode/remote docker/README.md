https://code.visualstudio.com/remote/advancedcontainers/develop-remote-host
这种方法可以不用在本地安装docker，只在远程开发机上安装即可

安装remote-ssh 和 dev-container插件
然后在remote explorer上就有两种选项：Remote/Container
使用方法是：
先用Remote的选项链接到服务器，然后再使用Container选项attach到容器上（好像除了attach，还有直接open folder的选项，暂时先选择open folder）

从小到大的几个维度：
editor是一个文件
folder是一个工作目录
docker
remote是一个远程机器
window是这个vscode软件

如果docker内目录打开错了，就应该close folder然后再open一个folder