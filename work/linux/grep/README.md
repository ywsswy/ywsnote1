find . -regextype egrep ! -regex '(.*tags)|(.*\.log)|(.*\.gcno)|(.*\.o)' -type f -exec grep -nHP --color=always -- 'guyihoid' {} \;
find . -regextype egrep ! -regex '(.*tags)|(.*\.log)|(.*\.gcno)|(.*\.o)' -type f |xargs --replace grep -nHP --color=always -- 'guyihoid' {}
以上两种写法的区别未知

其中 --表示参数结束，不然后面如果查找的内容含有--就会被误识别成参数了
其中 -exec是find的参数，而不是exec命令！
其中 {}和\;都是find命令-exec的参数要求，{}可以出现多次，每次执行命令都是把{}替换成find的一个文件名来执行

tail -f file |grep hello >>log #这种写法log为空
tail -f file |grep hello --line-buffered >>log #这种写法才行，另外如果多个grep串行，每个结尾都应该加上这个

-B<num> 表示同时显示grep到的这行的上面num行