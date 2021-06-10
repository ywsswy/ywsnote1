find . -regextype egrep ! -regex '(.*tags)|(.*\.log)|(.*\.gcno)|(.*\.o)' -type f -exec grep -nHP --color=always -- 'guyihoid' {} \;

tail -f file |grep hello >>log #这种写法log为空
tail -f file |grep hello --line-buffered >>log #这种写法才行，另外如果多个grep串行，每个结尾都应该加上这个

-B<num> 表示同时显示grep到的这行的上面num行