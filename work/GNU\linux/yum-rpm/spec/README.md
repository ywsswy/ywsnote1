自行打包的命令在SPECS目录执行rpmbuild -ba xxx.spec #同时产生rpm和srpm，-bb仅产生rpm
其执行原理是：
Executing(%prep)
Executing(%build)
1. 依照 *.spec文件内的Source来解压压缩包到BUILD目录
2. cd进入BUILD/"$Name"-"$Version"
3. 开始 %build 及 %install 的设置与编译
Executing(%install)
会自动find-debuginfo
生成到RPMS/SRPMS
Executing(%clean)


# 一些默认值（访问/echo可以使用%{dist}也可以%{?dist}，都要小写，不能大写
%{buildroot} $HOME/rpmbuild/BUILDROOT/"$Name"-"$Version"-"$Release"."$Architecture"
%{dist} 例如.el7

```
Summary: The NTP daemon and utilities
Name: ntp
Version: 4.2.6p5
Release: 29%{?dist}.2    #
SourceX： 源代码的tar包，要跟SOURCES下的文件名一致
Requires(post): 依赖性
Requires(preun):
Requires(postun):
Requires: # 安装rpm时所需要的软件
BuildRequires: #编译过程中所需要的软件

# 详细介绍
%description

# 编译前的预处理
%prep
%setup -q -a 5
%patch1 -p1 -b .sleep
sed -i 's|/var/db/ntp-kod|%{_localstatedir}/lib/sntp/kod|' sntp/{sntp.1,main.c}
for f in COPYRIGHT ChangeLog; do
    echo 'xxx'
done
%build #跟makefile很相关
%configure --enable-all-clocks --enable-parse-clocks
make %{?_smp_mflags}
# 安装过程要进行的操作
%install
mkdir -p %{buildroot}/usr/local/bin  #安装到用户机器上会把buildroot换掉
install -m 755 main %{buildroot}/usr/local/bin

%makeinstall -C ntpstat-*
install -p -m755 %{SOURCE7} .%{_libexecdir}/ntpdate-wrapper
%pre -n ntpdate
/usr/sbin/groupadd -g 38 ntp  2> /dev/null || :
%post
%systemd_post ntpd.service
%preun
%systemd_preun ntpd.service
%postun
%systemd_postun_with_restart ntpd.service

# 发布的文件
%files
%dir %{ntpdocdir}
%{_sbindir}/ntp-keygen

%changelog
* Tue Jun 23 2020 CentOS Sources <bugs@centos.org> - 4.2.6p5-29.el7.centos.2


```

其他
BuildRoot: 编译时，该使用哪个目录来暂存中间文件 （如编译过程的目标文件/链接文件）
