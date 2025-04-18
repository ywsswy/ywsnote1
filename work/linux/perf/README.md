## perf性能分析 & 火焰图


git clone https://github.com/brendangregg/FlameGraph.git
perf record -F 99 -p <pid> -g -- sleep 60  # 生成data文件
perf record -F 99 -p <pid> -g --call-graph dwarf -- sleep 60  # 能看到更详细的调用栈，但是后面生成火焰图html会很慢
perf report  # 命令行查看
perf script | ./stackcollapse-perf.pl | ./flamegraph.pl  > kernel.svg  # 生成火焰图html查看
