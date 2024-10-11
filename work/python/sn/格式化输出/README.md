https://blog.csdn.net/i_chaoren/article/details/77922939
【个人理想输出
    print(r'''Hello {} !!'''.format(name))
    print(r'''Hello {my} !!'''.format(my=name))

如果想输出{}则需要写成{{}}

id = '{head:0>5.2f}{tail}'.format(tail=id_tail,head=id_head)
0填充，右对齐，至少宽度为5，保留两位小数


还有一种格式化输出，就是使用%
"value:%s" % 1  # value:1