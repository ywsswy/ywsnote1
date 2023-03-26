倒排索引（单词->文档）
单词词典
倒排列表
TF 单词频率

|单词ID|单词|文档频率|倒排列表（DocID,TF,<POS>）|
|-|-|-|-|
|1|谷歌|2|(1,2,<1>),(2,2,<1>)|

单词词典的构造


- term是将传入的文本原封不动地（不分词）拿去查询。
- match【会对输入进行分词处理后】再去查询，部分命中的结果也会按照评分由高到低显示出来。
- match_phrase是按短语查询，只有存在这个短语的文档才会被显示出来

也就是说，term和match_phrase都可以用于精确匹配，而match用于模糊匹配

must既可以是array也可以是object
只有should 是array，中子句的满足一个即可

- Bool查询现在包括四种子句，must，filter,should,must_not
-- 返回的文档必须满足must子句的条件，并且参与计算分值
-- 返回的文档必须满足filter子句的条件。但是filter部分不参与计算分值
-- should也会计算分值


Elasticsearch的默认行为是把返回文档按它们的得分降序排列。
通过使用sort参数，可以指定自定义排序。
如果指定自定义排序，Elasticsearch将省略计算文档的_score字段，但是还是可以指定_score排序，来计算

fields
query
sort
from
min_score


query_string类似是写在url中的话
2.x版本用的是type:string, 5.x 用的是type:text
2.x的type=string，index=analyzed等于5.x的type=text，index=true

