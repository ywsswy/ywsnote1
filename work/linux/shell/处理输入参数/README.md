# sh a.sh -a -b hello  # getopts 仅支持短参数

```
# 初始化
a_flag=0
b_value=""

while getopts "ab:" opt; do
  case $opt in
    a)
      a_flag=1  # 无值flag
      ;;
    b)
      b_value=$OPTARG  # 有值参数
      ;;
  esac
done

echo "a_flag: $a_flag"
echo "b_value: $b_value"
```


# sh demo.sh --option1 ppp --option2 nnn  # getopt更强大还支持长参数

```
option1_value=""
option2_value=""

if [ $# -gt 0 ];then
  TEMPOPT=$(getopt -o '' -l option1:,option2: -- "$@")
  if [ $? != 0 ];then
    echo "Error parsing options"
    exit 1
  fi
  eval set -- "$TEMPOPT"
  while true; do
    case "$1" in
      --option1)
        option1_value="$2"
        shift 2
        ;;
      --option2)
        option2_value="$2"
        shift 2
        ;;
      --)
        shift
        break
        ;;
      *)
        echo "Unknown option: $1"
        exit 1
        ;;
    esac
  done
fi
```