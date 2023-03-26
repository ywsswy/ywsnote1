# 首先保证每一条命令都存盘在.bash_history找得到，修改~/.bash_profile如下
export PROMPT_COMMAND="history -a; $PROMPT_COMMAND"

# 其次在要查询历史记录前，从磁盘读取记录到内存 history -c; history -r