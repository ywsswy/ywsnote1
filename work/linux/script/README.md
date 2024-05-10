linux命令行中有些程序输出到stdout和输出到file的表现形式不同，例如输出到stdout是有颜色的，输出到文件是没有颜色的，因为程序会检测输出到的是否是终端，有什么办法欺骗/诱使程序相信其输出是一个终端吗：

script -e -q -c "<cmd>" /dev/null >res_file </dev/null


Q：（下面的问题已经解决了，因为有当前终端的输入干扰，所以只要加上`</dev/null`就可以了）
有没有类似bash -c的是不是也能达到类似的效果？
这个命令甚至有bug，把输入都清空掉了！！
这个命令的可能不会及时写文件？所以暂时的缓解方式是在执行完script >之后加上下述命令才能去处理输出文件，（或者尝试一下放到函数中？不过现在无法复现了。。。）：
```
        for((;;))do
                if [ -e res_file ];then
                        wc_file="$(wc -l res_file)"
                        wc_file_head="${wc_file:0:1}"
                        if [ "${wc_file_head}" != "0" ];then
                                break
                        fi  
                fi  
                sleep 0.001
        done
        sync
```