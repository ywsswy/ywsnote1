0)ubuntu首次使用要sudo apt-get install vim，或者install vim-gnome(wsl不支持)
默认vim -> vim.tiny
vim -> vim.basic，才支持语法高亮，自动补全之类的
vim-gnome -> vim.gnome，可以支持系统剪贴板复制粘贴，在以前的命令前加"+表示系统剪贴板

建立~/.vimrc文件(这个件是每次打开vim会自动执行的命令）
"双引号后面是注释
#[推荐写在.vimrc中]
set expandtab "把tab/缩进替换为空格。注意makefile的编写不能用空格，只能用tab，这时可以使用ctrl+v再输入tab键
set fileencodings=utf-8 "解决utf-8文件里面中文乱码
set hlsearch "高亮搜索词
set list "显示特殊字符
set makeprg=scons\ -j8"quickfix模式，自动跳到第一个出错的地方，写完代码不退出直接:make，然后出错后:cw可以打开错误提示，这种情况不要强制输出gcc的颜色
set noignorecase
set number "显示行号
set shiftwidth=2 "<>的缩进宽度
set tabstop=2 "设置tab宽度 "shiftwidth要和ts设置一致，如果shiftwidth宽度小于ts，那么往回缩的时候会额外加空格补齐了哇！
set wildmenu "显示一整行的提示符
autocmd FileType * setlocal formatoptions-=c formatoptions-=r formatoptions-=o "防止连续自动注释行
"syntax off "关闭语法高亮，默认是on
" 知其然不知其所以然，但是它是工具！
# 各种见识
set wrap "自动换行，但是vim -d 打开的文件貌似这个参数没生效
set scrollbind "在各自窗口输入这个，可以使得两窗口同时滚动
set noreadonly "🐮🍺
set fileformat=unix "把windows文件换为Unix文件
nnoremap ,,s ^istd::cout << <ESC>$a << std::endl;<ESC> #map命令后面不能写注释，设置按键映射，每次敲击,,s会执行的命令,n表示normal模式，nore表示不要递归执行
set iskeyword+=: #把冒号当作单词的一员，这样例如std::string就会被当成一个单词，-=是去掉
set binary #这个跟进入时使用vi -b效果一样
set noeol #在binary情况下，保存文件时不在文件尾加\n
set <key>? #查看一个设置项的值
set foldmethod #diff / manual
自定义函数https://cloud.tencent.com/developer/ask/29526

set paste "回车的时候会跟随上一行的注释的话，使用这个就避免从别处复制的程序变成注释而错位，但是粘贴模式无法在右下角看到当前位置
####################################################################
【编辑模式下】
Ctrl+i  移动到后一次位置
【命令模式下】
:g/pattern/d    #删除匹配pattern的行
:v/pattern/d    #删除不匹配pattern的行
:w !sudo tee %      强制保存
:w !clip.exe （可以把WSL中所有的内容复制到windows系统剪贴板），如果是选择某些行复制就进入可是模式进行该操作
:%!xxd  #以十六进制编辑（此时要用R替换模式）
:%!xxd -p -c16 #这种是可增删的十六进制编辑 用:%!xxd -r -p转回
:%!xxd -r 从十六进制恢复回正常模式
:r !command 运行命令把结果输入
:%normal jdd #删除偶数行

[{ 找上一层的{符号
]) 找下一层的)符号
ga  显示当前光标所在字符的码值
gj 向下一格（这是针对屏幕而言，不是针对代码行）
gu/Uw 单词变小/大写
~ 光标字母大小写变换
zf% 折叠这个花括号
zf<line>G 从这行折叠到<line>行
ctrl+r 反撤销

:set nobomb 设置utf-8务bomb模式（一般windows的php文件会有bomb头，这样网页可能显示异常）
:set bomb?  查询是否有bomb

"+y  复制到系统剪贴板
【复制粘贴类】
de：从光标处剪切至一个单子/单词的末尾，不包括空格

输出命令执行结果
<C-R>=expand("%:t") #当前文件名
<C-R>=expand("%:p:h") #绝对路径
<C-R>=expand("%:p") #绝对路径加文件名
<C-R>=strftime("%Y/%m/%d %H:%M:%S") #时间
vimrc中需要转义的 |

ctags可以快速查看定义（ctrl+]查看定义，ctrl+T返回）
ctags -R <file/folder> 提前建立tags文件
ctags -o ~/.vim/tags.include -R --c++-kinds=+p --fields=+iaS --extra=+q /usr/include #~/.vim/tags.include应该是vim默认能加载的

cscope可以查看所有使用处?
find . -regextype egrep -regex '.*\.((c)|(cc)|(cpp)|(h))' -type f >cscope.filescscope
cscope -Rbq
# :cs add cscope.out

【窗口操作】
<c-w>n 创建新窗口
<c-w><jkhl> 光标在窗口之间移动
<c-w><JKHL> 当前窗口浮动到最边缘
<c-w><num>- 变矮窗口
<c-w><num>_ 变最高窗口
<c-w><num>+ 变高窗口
<c-w><num>< 变窄窗口
<c-w><num>> 变宽窗口
<c-w><num>| 变最宽窗口
<c-x><c-f> 编辑模式时自动[补全/取消补全]文件路径<c-p><c-n>翻页
<c-w>f 打开当前光标所表示的文件路径