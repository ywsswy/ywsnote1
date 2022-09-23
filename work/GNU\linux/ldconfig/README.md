动态函数库
libxxx.so

需要先加载到内存中，程序才能调用


首先在/etc/ld.so.conf中指定要加载的配置

```
include ld.so.conf.d/*.conf
```
如上则只需在/etc/ld.so.conf.d目录里创建一个conf文件，内容为静态库所在的目录即可

然后使用ldconfig来读取更新缓存（到/etc/ld.so.cache中，ldconfig -p可以查看）


在/etc/ld.so.conf中指定 & ldconfig; 或者export LD_LIBRARY_PATH都可以