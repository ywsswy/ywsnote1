## 构建系统(工具)分类
- SCons
- autotools
- make
- cmake
- bazel


linux体系：内核、shell和文件系统构成了基本的操作系统，加上gnu提供的大量自由软件应用程序
驱动是内核中的一部分（或者说是内核的组件，是用来控制硬件的逻辑代码），从编程角度看内核就是一个调用库，其API函数提供给应用程序实现操作，同时内核与驱动通信实现对硬件的有效管理

绝大多数基于linux内核的操作系统使用了大量的gnu软件，包括了一个shell程序、工具、程序库、编译器及工具，还有许多其他程序
gnu计划的开创者richard stallman博士提议将linux操作系统改名为gnu/linux


linux版本：
- Debian (apt-get包管理)
- Ubuntu（基于Debian）：开源 & 免费；
- Fedora linux (dnf/yum包管理):第七版以前叫Fedora Core，由Fedora（开源）项目社区开发、red het公司赞助；在centos8之前，Fedora ==》red het ==》CentOS，就是说Fedora更像是一个小白鼠，稳定的功能加入red het，然后从red het 再拉出CentOS培养用户生态；CentOS8 之后，IBM（收购了red het公司）取消了CentOS，改为在Fedora 和 red het 之间滚动发布CentOS stream；
- CentOS：（Community-Enterprise-Operating-System）社区企业操作系统，免费；
- RHEL：（red het Enterprise Linux）red het 公司发布的面向企业用户的，收费；
- CentOS stream：没有centos稳定；所以有一些人选择其他的社区企业操作系统，比如RockyLinux；
- Mandriva
- Gentoo
- Slackware

## ps：
Enterprise
企业版，稳定，通常需要收费。可能部分不开源或受限。
Professional
专业版，类似企业版，但是可能技术服务支持力度不如企业版。
Community
社区版，开源，新功能或存在bug，待用户反馈。

FreeBSD:一种类UNIX操作系统
