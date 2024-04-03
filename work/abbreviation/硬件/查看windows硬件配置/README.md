# 我的电脑-》属性
## 处理器：Processor（eg：7th Gen Intel(R) Core(TM) i5-7500 1.7+ GHz）
- 12th Gen 表示12代
- i<x>-<y><...>中，x表示CPU等级i3表示入门，i5代表示主流，i7表示高端，y也表示代数，i5-7500就是主流7代，i7-12700就是高端12代
- 最后面还可能有H/K等后缀，K代表可以超频；H代表内部集成高性能显卡；

## 内存：Memory/RAM（eg：8GB）
- DDR3：速度低于2400MHz（不推荐了）
- DDR4：速度在2133~4400MHz
- DDR5：速度在4800MHz以上
## 操作系统：（eg：64-bit）

# Task manager-》performance
### CPU：（win+r msinfo32也能看）
- l1 cache、l2、l3；
- 插槽/Sockets：就是主板上物理插槽；数量通常表达成cpu（芯片）个数 or 处理器个数；大部分电脑就1个；
- 内核/Cores：数量通常表达成（物理）核（心）个数；（英特尔目前一个cpu最多15核），含大核+小核（频率不同）
- 逻辑处理器/Logical-processors(设备管理器里看到的处理器)：数量通常表达成cpu线程数（在软件层面or资源监视器层面的“cpu”个数）；可以比物理处理器数量多，因为用了超线程等实现的；（这里的线程是硬件层面的线程，不是软件层面的）

### 显卡（GPU）：这里可以看到有几张卡，属性分别有：
```
DirectX Version：（eg 12）
GPU memory/total memory = dedicated GPU memory + shard GPU memory
shared GPU memory
dedicated GPU memory/display memory(VRAM) 
```

# Win+R-》dxdiag
DirectX版本：（eg 12）
每一张显卡的配置

- 如何判断使用的是哪个显卡，一般运行不同的软件会自动选择哪张显卡，所以不确定的话可以在运行某软件时在任务管理器这里看各自资源使用率就知道了；我的电脑玩游戏用的哪一个，通常独立内存越高越好（dedicated），我玩2k的时候用的是AMD卡，而不是intel卡；
- 显卡对比网站：https://www.mydrivers.com/zhuanti/tianti/gpu/index.html
- 处理器对比网站：https://www.mydrivers.com/zhuanti/tianti/cpu/

ps:
英特尔除了酷睿（Core），还有至强（Xeon）处理器（主要面向服务器和工作站的），特点是低频多核
AMD有锐龙（Ryzen）