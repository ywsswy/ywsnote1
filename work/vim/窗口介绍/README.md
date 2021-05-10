Vim 编辑器底端 [noeol], [dos] 的含义

'noeol' 就是 'no end-of-line', 即“没有行末结束符”，Linux 下的文本编辑器（如 Vim）会在每一行 （包括最后一行）末尾添加一个换行符
cat -A hello-unix.txt命令可以看到这些换行符：
【noeol消除方法，:wq重新保存退出

【dos消除方法，Linux 下提供有两个命令用来进行 Windows 和 Unix 文件的转化：dos2unix 和 unix2dos;



Windows 和 Linux 下的换行符有两个明显不同：
Windows 下的换行符比 Linux 下的多了个 ^M;
最后一行行末没有换行符；


Windows 下文本文件每行的换行符为“回车+换行“(CRLF,^M$), 
 Linux 下则仅为 “换行” (LF, $). 