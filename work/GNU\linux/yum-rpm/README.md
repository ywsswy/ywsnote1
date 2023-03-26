(yum) Yellow dog Updater, Modified是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理器。基于RPM包管理

yum服务器上有软件依赖信息header
当用户有升级安装需求时，yum会把软件列表更新到本机/var/cache/yum中，跟本机RPM数据库（/var/lib/rpm/）比较确认需要下载哪些包

yum关于软件源的配置文件（/etc/yum.repos.d/里面的各个文件）中（$basearch变量通过arch命令获取，$releasever变量是rpm -qi centos-release的Version）
配置文件示例
```
[base]
# base是软件源的名字，必须存在，各文件中不能有相同的名字，yum install --enablerepo=<这里填软件源名字>
name=CentOS-$releasever - Base
baseurl=http://mirrors.aliyun.com/centos/$releasever/os/$basearch/
# 软件源网址，里面都会有repodata目录，存放了软件属性依赖
gpgcheck=1
# 是否要查看rpm数字签名
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
enabled=0
# 是否启用
```

#
yumdownloader pac # 下载包，但不安装，可加--enablerepo参数，rpm后缀可以不写，可写到arch
# rpm
rpm -qpR pac # 查看该包的依赖
rpm -qa pac* # 查看已安装了pac*没
rpm -ql pac 可以查看一个已安装的包在安装了哪些文件，如果有很多系统库中.h头文件说明他是个依赖库？（只有安装了的包才能执行这个命令）(如果有.pc文件可以从里面看到他依赖哪些库）
rpm -qf ul_sign.h 可以查看某个头文件是装哪个包装进来的
rpm --rebuilddb 重建数据库
rpm -e pac1 pac2 --nodeps 卸载，但不检查依赖~~
rpm -qpi pac --changelog #查看changelog
# yum
yum info pac #查看包的信息（如果已安装，用rpm -qi 可能更详细，还能看安装时间）
yum remove pac -y# 卸载，还有一种 rpm -e 。yum remove的时候，任何依赖方也会被删除
yum history # 查看历史操作
yum install <Package> -y # 安装，如果不是最新可以清理缓存
yum install <Package>-<Version>（不带release次数）
yum install <Package>-<Version>-<relase_time>.<Platform>
yum whatprovides <cmd> # 查询哪个软件包含了这个执行程序（未安装也能查出来）
yum deplist pac #查询一个包的依赖?
yum search <info> #搜索跟info相关的包，（包名，或者Summary中包含info的）
yum list pac* #这种可以
yum list installed |grep pac  #显示三列（包名 版本 在哪个软件源内）
yum repolist  # 列举被enable的软件源
yum list pac --showduplicates #查询一个包有哪些版本可以安装
yum reinstall pac重装当前版本包
yum update pac 升级包
yum clean all 清理缓存（软件源相关数据）
yum check-update pac 检查一个包是否可更新

## not recommended
yum check-update  # 更新到最新的消息
yum search pac #这种输入部分才能匹配到

# 相关命令
ldd # 查看二进制程序所依赖的库文件
rpm2cpio a-0.0.1-1.el7.x86_64.rpm | cpio -t # 查看rpm中有哪些文件
rpm2cpio a-0.0.1-1.el7.x86_64.rpm | cpio -idv ./home/service/app/a.sh # 把其中的文件提取出来

# rpm包命名规则
name-VERSION-release.arch.rpm
bash-4.2.46-19.el7.x86_64.rpm
release为19.el7就是第19次发布，基于redhat enterprise linux 7操作系统
x86_64为针对64位CPU进行的优化编译设置的平台(苹果公司和rpm用的这个，甲骨文和微软用的x64，BSD和其他linux用的amd64，32位则叫i386)
i686
noarch为没有硬件限制的平台，常为shell脚本

还有xxx.src.rpm，是包含源代码的包（在updates-source软件源中找下载地址吧）一般是拿去做正式rpm用的redist包，并不能代表真正的源码，最“源”代码的应该还是要去git/官网仓库里面下载，另外还有一个tarballs的概念/tar.gz
直接sudo rpm -ivh xxx.src.rpm会在home创建rpmbuild目录，里面有SPECS/SOURCES文件夹，能找到spec文件

