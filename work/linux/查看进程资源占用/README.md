# top命令看到的属性
VIRT	进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES
RES	进程使用的、未被换出的物理内存大小，【单位kb】。RES=CODE+DATA

# /proc/\<pid>/statm 文件
第一列是VmSize (pages)  数值等于top中的VIRT/4
第二列是VmRSS (pages) 数值等于top中的RES/4

# OOM之后使用命令dmesg －T | grep -E -i -B100 'killed process'
```
total_vm相当于VmSize
rss相当于VmRSS
 [ pid ]   uid  tgid total_vm      rss nr_ptes swapents oom_score_adj name
[13478742.063648] [ 2748]     0  2748    30551     1935      19        0             0 bash
[13478742.065166] [ 2805]     0  2805    32156      285      22        0             0 screen
[13478742.066692] [ 2806]     0  2806    36579     4749      25        0             0 screen
[13478742.068223] [ 2807]     0  2807    30572     2018      20        0             0 sh
[13478742.069700] [ 2839]     0  2839    30572     2022      19        0             0 sh
[13478742.071169] [ 5228]     0  5228    39653      534      35        0             0 top
[13478742.072673] [ 6346]     0  6346   255013   246895     495        0             0 a.out
[13478742.074181] Memory cgroup out of memory: Kill process 6346 (a.out) score 966 or sacrifice child
[13478742.075788] Killed process 6346 (a.out) total-vm:1020052kB, anon-rss:986336kB, file-rss:1244kB

```

# 代码中的API
<unistd.h>中getpagesize();函数可以计算本机一个page多少kb，一般是4096



平均负载要结合核数来看
负载为2，
如果1核，表示有一半的进程抢不到CPU
如果2核，表示CPU刚好被完全占用
如果4核，表示CPU有50%的空闲
建议值，不要超过核数x0.7

## 但是负载高，不表示CPU使用率高
比如I/O密集型的，拉高了负载，却不一定拉高CPU使用率