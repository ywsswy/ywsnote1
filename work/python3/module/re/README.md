cmd = 'curl -XGET -s $ipaddr/$index'
    cmd = re.sub('\$ipaddr', YGlobal.args.ipaddr, cmd)
    cmd = re.sub('\$index', YGlobal.args.index, cmd)


phone = "2004-959-559 # 这是一个国外电话号码"
 

num = re.sub(r'#.*$', "", phone)

# 把警号以及后面的都删除
//////////////////////////////////////////////////////////////////////

import re
m = re.search('(?<=abc)def', 'abcdef') #表示搜索但是不放到结果中
m.group(0)
'def'