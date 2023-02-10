# /sys/fs/cgroup/目录
## 关于内存：
cat /sys/fs/cgroup/memory/memory.limit_in_bytes 显示节点被分配的内存数（单位 字节），但这个值可能没设置/特别大，目前只在k8s上见过
某进程的内存使用情况（以后再说）


## 关于CPU：
cat /sys/fs/cgroup/cpu/cpu.cfs_quota_us 显示cpu的时间分配限制，有些k8s的容器里可以通过这个来指定分配的cpu核数，如果是-1则表示不受限制
cat /sys/fs/cgroup/cpu/cpu.cfs_period_us 显示1核cpu满载的最大分配数，通常通过/sys/fs/cgroup/cpu/cpu.cfs_quota_us除以/sys/fs/cgroup/cpu/cpu.cfs_period_us就可以算出节点被“分配”的的cpu核数【c】
cat /sys/fs/cgroup/cpuacct/cpuacct.usage k8s容器内的cpu使用情况【u】
usage = (【u2】 - 【u1】) / (t2 - t1) / 【c】 一段时间内容器cpu使用率，见下方的示例脚本

# Q 

跟/proc/\<pid>/statm有什么区别


```
#!/usr/bin/env bash
DOCKER_CPU_QUOTA=`cat /sys/fs/cgroup/cpu/cpu.cfs_quota_us`
DOCKER_CPU_PERIOD=`cat /sys/fs/cgroup/cpu/cpu.cfs_period_us`
CPU_RATIO=`echo "${DOCKER_CPU_QUOTA}/${DOCKER_CPU_PERIOD}" | bc -l`
u1ns=`cat /sys/fs/cgroup/cpuacct/cpuacct.usage`
t1ns=`date +"%s%N"`
for ((;;))do
        sleep 3
        u2ns=`cat /sys/fs/cgroup/cpuacct/cpuacct.usage`
        t2ns=`date +"%s%N"`
        s5=$(echo "($u2ns - $u1ns) / ($t2ns - $t1ns) / $CPU_RATIO" |bc -l)
        echo $(date +"%Y-%m-%d %H:%M:%S")" ""$s5"
        u1ns=$u2ns
        t1ns=$t2ns
done
```