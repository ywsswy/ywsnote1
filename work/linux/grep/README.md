find . -regextype egrep ! -regex '(.*tags)|(.*\.log)|(.*\.gcno)|(.*\.o)' -type f -exec grep -nHP --color=always -- 'guyihoid' {} \;

tail -f file |grep hello >>log #这种写法log为空
tail -f file |grep hello --line-buffered >>log #这种写法才行