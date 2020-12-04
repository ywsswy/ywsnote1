zkCli.sh -server <ip>:<port>

h #显示所有命令

rmr <path> 删除路径节点及其所有子节点


# 这是一种可以用历史记录修改命令的方法
zkCli.sh -server <ip>:<port> <<EOF
ls /
quit
EOF