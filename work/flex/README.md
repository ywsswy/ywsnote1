行读取读取到YY_CURRENT_BUFFER_LVALUE->yy_ch_buf里

p * (yy_buffer_stack)[(yy_buffer_stack_top)]

正常的行读取后跳到EOB_ACT_CONTINUE_SCAN，然后do yy_match



几个宏
yytext_ptr
yytext

## stat

初始stat是2

## static定义
### yy_ec（EOF 0 \n 2 数字 3 其他 1）#值不同，size相同，但是每个取值不同，取值的种类等于yy_meta的size
### yy_accept {0, 0, 0, 4, 1, 3, 1, 0, 0, 2, 0} #不同的匹配下，生成的yy_accept是不同的

## 普通字符需要转义的有：- 空格

## 一些匹配方法
<<EOF>> 匹配结束符，别忘了处理完return 

## 多个正则均匹配的时候，优先读取哪一个？

[0-9]{4} > [0-9]{3}  #即2567777会被当作一个2567 + 777

[0-9]{4} = ^[0-9]{4} = 3333 #同级(长度固定且相同），则看谁在上就先匹配，这里如果把[0-9]{4}写在上面，^[0-9]{4}则永远不会被匹配了

[0-9]{4} > 34 #固定长度的，越长优先级越高

.* >= 2314982175.*
> .