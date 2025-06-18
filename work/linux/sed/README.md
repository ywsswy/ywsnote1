sed -i.bak s/-O3/-O2/g hello.txt # 把所有的-O3替换成-O2

-i.bak 表示原文件做备份，加了-i参数必须有输入文件，不加-i才可以从标准输入读
-r 表示支持正则表达式（但是sed的正则比较老，建议用perf命令做正则替换！！）
替换方法类似vim，（只不过vim里面要%s表示所有行，s表示当前行）

addr/regexp/replacement/flag

### addr：
<num> 第几行
<start>,<end> 区间范围[]
<start>~<step> 起始+步长
<start>,+<num> [start,start+num] #,和~可以结合使用
s 所有行

### flag
g表示该【行】的全部匹配项都处理，不加表示只处理该行的第一个

## 其他command
sed -i.bak '5d' file #把第5行删掉
sed -i.bak '5i\context' file #把内容插入第5行前，原第5行会到第6行
sed '1,/^$/d'# 把第一行（含）到第一个空行（含）之间的内容删掉
sed -i /^http_proxy=/d'  # 把http_proxy=开头的行删掉

## 备注：
```
sed -n '5p;5q' note.txt #可以加快sed的执行速度，遍历到第5行就退出了。
# 同样的写法还一下两种，普遍来看sed效率最高
head -n 5 note.txt |tail -n 1
awk '{if (NR==5){printf("%s\n", $0);exit;}}' note.txt
```


## 这个报错，是没找到结束符，可能是缺/，可能是有非法不可见字符在干扰，打印到文件里 hexdump瞧瞧吧
sed: -e expression #1, char 33: unterminated `s' command

## 这个报错，是正则替换有问题哦，
sed: -e expression #1, char 10: invalid reference \1 on `s' command's RHS
