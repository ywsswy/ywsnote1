## perf性能分析 & 火焰图


git clone https://github.com/brendangregg/FlameGraph.git
perf record -F 99 -p <pid> -g -- sleep 60  # 生成data文件
perf report  # 命令行查看
perf script | ./stackcollapse-perf.pl | ./flamegraph.pl  > kernel.svg  # 生成火焰图html查看