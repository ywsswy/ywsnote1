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

# sh a.sh -a -b hello