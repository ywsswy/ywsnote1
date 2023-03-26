1 漏洞挖掘利用综述
2 栈溢出
	1 c函数的汇编形式
	2 一些基本概念 shellcode payload 
	3 最经典的栈溢出
	4 开启了ASLR的栈溢出
3 初级ROP
	1 ROP介绍
	2 ROP模块介绍
	2 ret2text
	3 ret2plt			ppt
	4 ret2libc			
	4 ret2syscall
	5 构造rop链		ppt
*4 高级ROP
	1 64位ROP ---补充64位参数传递
	2 stack pivot
	2 ret2dl-resolve 	http://www.evil0x.com/posts/19226.html
	4 srop
	7 dynelf泄露内存总结 (write puts 栈平衡)
5 格式化字符串漏洞
	1 格式化字符串函数
	2 格式化字符串
	3 格式化字符串漏洞原理
	4 利用实例
		1 读内存 ----例子 绕过canary
		2 写内存----例子 GOT覆写 基本利用 
		任意写内存的一些目标
	5 更复杂的漏洞情况
	7 fmtstr
6 堆漏洞一
	1 堆概述
	2 堆相关的数据结构
	3 fastbin溢出
	4 fastbin double free
	
7 堆漏洞二
	house of spirit
	house of force
		
	补充:什么时候分配的chunk会使用下一个chunk的prev_size字段当作数据段
	补充one-gadget rce
	house_of_einherjar
	
8 堆漏洞三
	unlink
		1 古老的unlink
		2 现代unlink
			堆溢出
				前向合并
				后向合并 
			bins double free 必须存在UAF
	
	UAF 
		功能
			泄露信息
			修改元数据
		补充:虚函数
		虚函数 pwnable.kr的例子
	
9 堆漏洞四	
	重叠堆			
		扩展堆
		缩小堆
	
	
	unsorted bin attack		
			https://www.anquanke.com/post/id/85127
			http://tacxingxing.com/2017/09/17/unsorted-bin-attack/
	off by one		
		https://www.anquanke.com/post/id/84752
		利用的条件
		利用效果
			重叠堆
				off by one overwrite allocated
				off by one overwrite freed
				off by one null byte
			unlink
			 	off by one small bin
	house of orange
		https://bbs.pediy.com/thread-222718.htm
		https://www.anquanke.com/post/id/84965
		http://tacxingxing.com/2018/01/10/house-of-oreange/
		https://bbs.pediy.com/thread-223334.htm
10  利用技巧补充
	1 jmp esp
	2 Pwngdb
		https://github.com/scwuaptx/Pwngdb
	 6 setbuf (例题)
	2 整数溢出 
	7 canary绕过
		内存泄漏
		一字节一字节覆盖(条件:手动写网络部分)
		c++的异常捕获机制
		stack smash		 
	10 新版本libc下IO_FILE的利用
	5 各个函数的截断问题
	8 程序修复方法 pwntools的ELF模块

