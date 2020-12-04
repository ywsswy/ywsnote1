【自定义映射】
map <命令模式下输入的命令> <触发的自动化操作>
# vim有六种模式，这六种映射分别在哪种模式中起作用如下表
|.vimrc中的映射命令|Normal 常规模式|Visual 可视化模式|Operator Pending 运算符模式|Insert Only 插入模式|Command Line 命令行模式|
|---|---|---|---|---|---|
|map|√|√|√|
|map!|-|-|-|√|√|
|nmap|√|	 	 	 
|vmap|-|√|	 	 	 
|omap|-|-|√|
|imap|-|-|-|√|-|
|cmap|-|-|-|-|√|
	
	

<Esc>
<F4>
<C-o> "ctrl+o

命令模式下%表示当前文件名