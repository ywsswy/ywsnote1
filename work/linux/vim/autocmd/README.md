autocmd event1,event2 patten cmd
autocmd BufNewFile *.txt :w
当新打开一个txt文件时，就在打开后执行:w
其他event：
BufWritePre 
before you write any file
BufRead * :normal gg=G
open a file, this only change view don't change the disk