# 单独开线程，设置超时控制

function WaitFunction() {
  timeout="$1"
  {
    trap 'kill $(jobs -p) >&2' EXIT;
    # TODO process lock

  }&
  sub_pid=$!
  sleep $timeout &
  wait $!
  kill $sub_pid
}

res=$(WaitFunction 4)