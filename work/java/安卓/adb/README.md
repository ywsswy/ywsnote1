adb devices # 查看有哪些设备（输出的第一列是<设备id>）
adb -s <设备id> root # 获取root权限
adb -s <设备id> remount # 相当于 ‘-o rw,remount,rw /system’ 有些系统文件夹下的操作必须要这样，不然是只读的
adb -s <设备id> reboot # 重启（每次重启后都需要重新remount）
adb -s <设备id> push <pc_file> /sdcard/Download/ # 把电脑文件传输到安卓的下载文件夹
adb -s <设备id> pull /sdcard/Download/<phone_file> .
adb -s <设备id> shell # 指定进入一台安卓，然后在安卓上执行命令，exit可退出
adb -s <设备id> shell <cmd> # 指定一台安卓，执行命令

### 安卓上的命令列表
pm list package
dumpsys window | findstr mCurrentFocus
getprop ro.build.version.release  # 例如 10
getprop ro.product.cpu.abi  # 查看CPU架构 例如 arm64-v8a

## CPU架构包括 armeabi，armeabi-v7a，x86，mips，arm64- v8a，mips64，x86_64
```
• mips / mips64: 极少用于手机可以忽略（谷歌最新的文档已经不支持了）
• armeabi: ARM v5 这是相当老旧的一个版本，缺少对浮点数计算的硬件支持，在需要大量计算时有性能瓶颈
• x86 / x86_64: x86 架构的手机都会包含由 Intel 提供的称为 Houdini 的指令集动态转码工具，实现 对 arm .so 的兼容，约1% 以下的市场占有率
• armeabi-v7a: ARM v7(32bit, armv7)，比较主流
• arm64-v8a: （aarch64, armv8)，非常主流
```


## 其他

adb logcat | find "LocationManager"
adb install <apk 绝对路径>

可以结合scrcpy在电脑端控制手机