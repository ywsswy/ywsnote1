http://www.cnblogs.com/wangqiguo/p/5793448.html
[others

-b  --ignore-space-change  Ignore changes in the amount of white space.
-B  --ignore-blank-lines  Ignore changes whose lines are all blank.
-q  不显示diff内容，只判断是否有diff（有则返回码非0）


diff -ruq <old file> <new file> #仅输出文件名

diff -ru <old file> <new file>
--- <old file>
+++ <new file>
@@ -<old line number>,<old line length> +<new line number>,<new line length> @@
- <delete line content>
+ <add line content>
 <unchanged line content>
@@ ......//第二处改动，是在第一处改完后的情况下的diff



【例如】
[old.txt]
0
1
2
[new.txt]
-2
-1
0
1
diff -u old.txt new.txt
@@ -1,2 +1,4 @@ //先写出old.txt从第1行开始共2行的内容，然后经过add/delete，之后你看到的就是new.txt从第1行开始共4行的内容
+-2
+-1
0
1




