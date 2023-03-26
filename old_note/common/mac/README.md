safari修改并重发网络请求（请求一次，在网络中右键create request local override，然后编辑好，再重新请求一次，虽然控制台看好像没改，但是实际是改了的。。。实在觉得难用就用firefox吧）
chrome全屏 command + control + f

m4a转mp3
ffmpeg -i inputfile.m4a -acodec libmp3lame -ab 93k outputfile.mp3

command + tab能切换
command + f 能查找
command + v 粘贴
command + 左/右 terminal窗口间切换
command + 左 home ....
鼠标停在该显示屏最下方几秒 程序坞显示到该显示屏
command + shift + 4 截屏
command + delete 删除文件
command + 上 在finder界面进入上一层目录
ctrl + space 切换输入法

Fn + 左/右/上/下 home/end/pageup/pagedown

只有左边有control
复制粘贴尽量两个手指距离近一点

大部分是ommand，纯正的是control

更新screen版本


VScode+golang cmd+左键 跳转 //ctrl+- 回跳


terminal设置，preference里面profiles选择pro，并设置为默认，勾选Antialias text，opacity设置100%

自带录屏quicktime，无法录制系统内部声音，可以安装loopback，修改sources中的输入（麦克风要选，app应用要选）即可，quicktime的输入设置成loopback！

killall Dock  # 重启dock，解决有时候程序坞不显示程序的问题

当打开没有签名的 Mac 应用时，可能会报 “App can’t be opened because it is from an unidentified developer” 的错误。这种安全机制叫做 GateKeeper。删除这个属性，就可以去除app 的隔离性，实现打开软件。
>xattr -d  com.apple.quarantine <targetapp>