/proc/<pid>/statm 文件
第一列是VmSize (pages)  数值等于top中的VIRT/4
第二列是VmRSS (pages) 数值等于top中的RES/4


VIRT	进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES
RES	进程使用的、未被换出的物理内存大小，【单位kb】。RES=CODE+DATA

<unistd.h>中getpagesize();函数可以计算本机一个page多少kb，一般是4096



平均负载要结合核数来看
负载为2，
如果1核，表示有一半的进程抢不到CPU
如果2核，表示CPU刚好被完全占用
如果4核，表示CPU有50%的空闲
建议值，不要超过核数x0.7

## 但是负载高，不表示CPU使用率高
比如I/O密集型的，拉高了负载，却不一定拉高CPU使用率