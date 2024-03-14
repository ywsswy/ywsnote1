## cpu

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

## 内存

#!/usr/bin/env bash
DOCKER_MEM_LIMIT=`cat /sys/fs/cgroup/memory/memory.limit_in_bytes`
for ((;;))do
        DOCKER_MEM_USAGE=`cat /sys/fs/cgroup/memory/memory.usage_in_bytes`
        DOCKER_MEM_INACTIVE=`cat /sys/fs/cgroup/memory/memory.stat |grep total_inactive_file |perl -pe 's|.* (.*)|\1|'`
        DOCKER_MEM_ACTIVE=`cat /sys/fs/cgroup/memory/memory.stat |grep total_active_file |perl -pe 's|.* (.*)|\1|'`
        s5=$(echo "($DOCKER_MEM_USAGE - $DOCKER_MEM_INACTIVE - $DOCKER_MEM_ACTIVE) / $DOCKER_MEM_LIMIT" |bc -l)
        echo $(date +"%Y-%m-%d %H:%M:%S")" ""$s5"
        sleep 3
done