默认大家是共用一台机器，所以自己是没有root用户的

有时候不需要在docker内开发，有时候需要

普通用户本身需要免密ssh登陆
bazel等需要免密ssh拉代码


vscode插件需要能登陆到容器内，编辑文件【可以考虑？】


普通用户自己的：
$HOME/.ssh


遇到的问题：所以不是root用户，，，我就放弃了（太难了，母机创建的user:group，docker内看就成了1002:1003，docker内创建的，母机看就成了固定的docker安装用户）
1）docker内如果是root用户，没有权限访问母机的$HOME/.ssh，拉代码失败
2）docker内如果是root用户，并且vscode是连接母机普通用户，无法编辑docker内的文件，
3）docker内如果是root用户，docker内保存的文件，母机普通用户反而无法读写

docker的一个思路是root随意创建，进docker之后切普通用户，但是不确定能vscode登陆


ps:

vscode使用docker的话，需要本地物理机也安装docker