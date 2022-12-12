# .bash_profile
# Get the aliases and functions
if [ -f ~/.bashrc ];then
  . ~/.bashrc
fi

# .bashrc
# Source global definitions
if [ -f /etc/bashrc ]; then
  . /etc/bashrc
fi
# 环境变量：不export了话，这个变量只能在当前shell下使用，在shell的子进程中无法使用
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$HOME/software/gc/lib/:.:$HOME/lib:$HOME/lib64:$HOME/libexec"
export LC_CTYPE="zh_CN.UTF-8"
export LANG="en_US.utf-8"
export PATH="$PATH:$HOME/.local/bin:$HOME/bin:$HOME/workspace/github.com/ywsswy/shell:$HOME/software/hadoop/bin:/usr/sbin:/sbin:/usr/local/bin:/usr/local/go/bin:$HOME/software/git-2.23.0/bin:$GOROOT/bin:$GOPATH/bin:$HOME/.ft:/home/hill/software/git-2.23.0/bin:$HOME/Library/Python/3.8/bin"
# 默认history并不记录时间，除非使用HISTTIMEFORMAT。
export HISTTIMEFORMAT="%F %T "
# .bash_history的文件最大行数
export HISTFILESIZE=5000000
# history显示的最近记录条数
export HISTSIZE=20000
export PROMPT_COMMAND="history -a; $PROMPT_COMMAND"
# 默认这次登陆的历史记录会在退出会话的时候保存到~/.bash_history中，除非history -a能保证实时写入
export TERM=xterm
# 防止timed out waiting for input: auto-logout超时等待输入
export TMOUT=0
ulimit -c unlimited
# 不用--color=auto是因为mac不支持
alias ll='ls -l --color=auto'


service cron start #wsl


## 
/etc/profile #基本没有什么改动
/etc/bashrc #也不要加prompt_command啦
~/.bash_profile保证要存在，首先调用~/.bashrc，然后写自定义环境变量
~/.bashrc 首先调用/etc/bashrc

- 交互式的登陆shell，例如（ssh <user>@<host>）
- 交互式的非登陆shell，例如在一个已有shell中运行bash，或者screen命令（这种虽然不会执行~/.bash_profile，但是因为没有涉及到登陆，原有的环境变量还是存在着的，个别变量除外）（且screen并不是在上一个screen基础上执行~/.bashrc，而是在顶层shell的基础上执行）
- bash的每种模式会读取其所在列的内容，首先执行A，然后是B，C。而B1，B2和B3表示只会执行第一个存在的文件
+----------------+--------+-----------+---------------+
|                | login  |interactive|non-interactive|
|                |        |non-login  |non-login      |
+----------------+--------+-----------+---------------+
|/etc/profile    |   A    |           |               |
+----------------+--------+-----------+---------------+
|/etc/bash.bashrc|        |    A      |               |(ubuntu有这个文件，centos好像没有了）
+----------------+--------+-----------+---------------+
|~/.bashrc       |        |    B      |               |
+----------------+--------+-----------+---------------+
|~/.bash_profile |   B1   |           |               |
+----------------+--------+-----------+---------------+
|~/.bash_login   |   B2   |           |               |
+----------------+--------+-----------+---------------+
|~/.profile      |   B3   |           |               |
+----------------+--------+-----------+---------------+
|BASH_ENV        |        |           |       A       |
+----------------+--------+-----------+---------------+


## ~/.inputrc
"\e[5~": history-search-backward
"\e[6~": history-search-forward