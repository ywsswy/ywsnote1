- [demo&安装(wsl sudo 1.4版本静态安装成功)](https://www.cnblogs.com/cswuyg/p/10402218.html)
./configure --prefix=/usr/local/app/xapian/install --enable-static=yes --enable-shared=false
sudo make install
export PATH=/usr/local/app/xapian/install/bin
/mnt/d/workspace/cpp/testxapian

- [示例代码](git@github.com:xapian/xapian-docsprint.git) make html LANGUAGE=c++生成C++文档
- [默认的trem prefix](https://xapian.org/docs/omega/termprefixes.html)

# 概念
分组
文档did
- wqf 词频


使用上，自己写配置文件加载类，按照字段分别建立不同索引
空间索引不想使用geo_hash了（确认原来使用的是啥）


# 关注点
- 索引 query 排序 
- 示例程序
- 是否满足现有需求
- 与BS的异同点
- Xapian没有Field的概念？
- 性能
- 哪些features需要关注
- 如何分片？

### 数据类

- Document # Xapian::Document::Internal中的方法api/omdocument.cc
           # add_value(Xapian::valueno slot, const string &value)  # 内部的value是个map<slot, string>
           # set_data() #仅仅是设置好二进制，原封不动取出
           # add_term()
           # add_posting()
           # add_boolean_term(string unique_id) # 添加doc的唯一区分term，更像是一个标识符，这种term无需存frequency信息

-- 查询阶段
- Query # 单个term string "T<term>"
        # 多个term enum op, Query q1, Query q2
        # get_description() #可视化当前query
- Database

- Enquire #用Database初始化，存放Query查询条件、KeyMaker排序、vector<MatchSpy *>分组，返回MSet查询结果

- MSet #最终查询结果，提供了迭代器访问数据




backends目录下的源码吧，这里面都是实际功能的实现

xapian使用了pimpl的设计模式
有chert，barss,flint（默认）三种数据模式/存储格式，访问flint的Document_values比chert快

XAPIAN创建数据库后主要有如下7张表，相对而言前面三张表是重点 。
1. posting listtable 保存了被每一个term索引的document，实际上保存的应该是document在database中的Id，此Id是唯一的。
2. record table 保存了每一个document所关联的data，data不能通过query检索，只能通过document来获取。
3. term list table 保存了索引每个document的所有的term。
4. position listtable 保存了每一个Term出现在每一个document中的位置。
5. value table 保存了每一个document的values，values是用作保存、排序或其它作用的。
6. spellingtable 保存了拼写纠正的数据。
7. synonymtable 保存术语的字典，例如NBA、C#或C++等。

给定一个名为D的document，存在着一个terms列表索引着它，我们称之为D的term list。（文档的term信息）
给定一个名为t的term，它索引着一个documents列表，这称之为t的posting list。（倒排链）

- Xapian的 Terms和Documents都是使用B-树来存储的，具有增删改查比较方便迅速的特点，缺点则是如果索引被删除后的空间不能重复利用，为了提高性能，通常要经常重建索引。

Documents:
  data //仅二进制，存储返回给用户显示，其他啥也不干，所以无用字段要放在这里，降低性能开销
  terms
  values //在排序？等过程中可能会用到的一个个值，类似正排brief字段？每个字段的值存放在一个槽位（slot）？对此的访问可以高效得应用过滤
Stemmers: //词根 文本规范化（但是只支持英文，中文不行吧）
xapian.TermGenerator//建索引时把string分成term
QueryParser//查询时把用户语法转换成查询Query（author:"william shakespeare" title:juliet 会被解析成什么？）

docid：每个存储在Xapian数据库中的文档都有一个指定的或者自增的不重复id值，（但查文档是用term的，无法通过docid找到文档）

- Term Prefixes: X starts a multi-capital letter user-defined prefix。指定字段查询（但是也只是字符串的term）

# 其他特性
- xapian支持远程索引库，也支持一个索引库拆分成多个子索引库；
- inmemory remote chert glass
- 可以对数据进行检查
xapian-delve <folder>
xapian-delve -r 2 -d <folder> # r查看第几个文档的termlist，d查看
xapian-delve -t Q1974-100 <folder> # t 查看哪些文档有某个term