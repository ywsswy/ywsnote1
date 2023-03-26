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
getprop ro.product.cpu.abi # 查看CPU架构


## 其他

adb logcat | find "LocationManager"
adb install <apk 绝对路径>

可以结合scrcpy在电脑端控制手机