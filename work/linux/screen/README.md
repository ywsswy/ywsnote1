screen 在当前screen中创建一个终端连接，如果不在screen中，则会先创建一个screen
screen -d # 回到普通终端，不在screen中。
screen -ls ==>显示有几个screen以及状态，29008.pts-2.VM-2-11-ubuntu中就有了PID(29008)
screen -r <PID> # 进入到某个screen中
在screen中exit即可结束此终端连接，如果一个screen中所有的终端连接都退出了，则这个screen就消亡了
screen -p 2 -X stuff "pwd^M" #往第2个窗口执行pwd命令，这可以用于批量执行


screen的Detached状态表示这个screen下没有任何其他bash？
Attached表示至少有一个bash在里面连接中

c-a + " 进入screen中的终端列表，Ctrl + p/n 即可选择使用哪个终端
c-a + ' 输入想进入的终端
c-a + : 执行命令，例如.screenrc中的

c-a + c 创建一个新窗口并切换到其
c-a + k 删除该窗口
c-a + S 上下分两个窗口
c-a + | 左右分两个窗口
c-a + tab切换上下窗口
c-a + X 删掉这个屏幕分区
c-a + Q 删掉其他屏幕分区

c-a + c-[ 进入用选择复制模式/非insert模式（enter开始复制）
c-a + ] 粘贴

c-a a 相当于平时的c-a
c-a i 查看当前窗口的信息

# .screenrc
altscreen on
defscrollback 20000  #scrollback命令可以对当前窗口即时生效
vbell off  # 非视觉提醒，而是声觉提醒，这样就可以蜂鸣器叫了。。