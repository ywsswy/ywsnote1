adduser在使用该命令创建用户是会在/home下自动创建与用户名同名的用户目录，passwd命令修改其密码(bingo)。-d <HOME_DIR>
userdel -r 删除用户及相关目录。


cat /etc/passwd，可以看每个用户的信息：用户名:x隐藏密码:用户标识uid:所属主组标识gid:说明:该用户home目录:启动shell进程
- sam:x:200:50:Sam san:/home/sam:/bin/sh

cat /etc/group，可以看到每个group的组标识，以及哪些user挂到这个组下

一个组中可以有多个用户，一个用户也可以属于不同的组。
用户要访问属于附加组的文件时，必须首先使用newgrp命令使自己成为所要访问的组中的成员

id <username> 还能查看所属的其他附加组
- uid=1000(hill) gid=1000(hill) groups=1000(hill)

usermod -a -G <newgroupname> <username> 把一个用户添加到某个附加组中，此时这个用户还不能直接访问，要重新登陆或者还需要他执行newgrp <newgroupname>登记一下？
不能用~gpasswd -M <user> <group>~ ，因为这个修改会把group覆盖

groupadd 创建用户组

groupdel 删除用户组
