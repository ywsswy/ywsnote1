
# 这是起两个进程（包含一个超时等待进程）
# 如果任一进程完成则不等其他进程
# 再加多个进程时，写法类似

function WaitFunction() {
  timeout="$1"
  {
    trap 'kill $(jobs -p) >&2' EXIT;
    sleep $timeout
  }&
  wait_pid=$!
  {
    trap 'kill $(jobs -p) >&2' EXIT;  # 如果这个语句块内进程到了超时时间还没有结束，外面会发kill命令，这个trap就能收到信号“自杀”，如果没等收到信号就结束了的话，此时kill空有报错提示也正常
    # TODO process work
  }&
  sub_pid=$!
  for((;;))do
    kill -0 $wait_pid 2>/dev/null
    rec=$?
    if [ $rec -ne 0 ];then
            kill $sub_pid
            break
    fi
    kill -0 $sub_pid 2>/dev/null
    rec=$?
    if [ $rec -ne 0 ];then  # 一个进程结束则不等了，如果要求都完成，可以在这里补充并且条件
            kill $wait_pid
            break
    fi
    sleep 1
  done
}

res=$(WaitFunction 4)