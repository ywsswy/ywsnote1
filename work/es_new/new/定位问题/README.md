# 被召回的文档得分是怎么计算的
"explain":true

# 文档是否被匹配，以及原因

GET /index/_explain/<id>

常规就是hits.hits[*]._source下面就是各个结果
加了explain之后，还会有hits.hits[*]._explanation下面会有一层层的value，description，details[]
同一层的description解释了value是什么的值，以及如果利用details中的参数算出来的value
其中，description可以有如下格式
## weight(title:哪里 in 173) [PerFieldSimilarity], result of:
一种算法方式，就是details里面score的分数
## score(doc=173,freq=1.0 = termFreq=1.0\n), product of:
表示由details中的各个参数相乘得来
## boost
表示value就是各单纯的参数，没有再里层的details了
## idf, computed as log(1 + (docCount - docFreq + 0.5) / (docFreq + 0.5)) from:
docFreq 1 包含改词的文档数
docCount 281 总文档数
idf = l(1 + (281 - 1 + 0.5) / 1.5) = 5.2364419
## tfNorm, computed as (freq * (k1 + 1)) / (freq + k1 * (1 - b + b * fieldLength / avgFieldLength)) from:
tf归一化值，idf是针对检索结果记录数与总记录数之间的关系来计算的得分，而归一值则是通过计算单个文档内部的查询结果进行打分；在ES中，针对不需要分词的字段(keyword)类型的默认是关闭归一值的
termFreq=1.0
parameter k1 饱和度值，默认值为1.2
parameter b 长度归一化参数，默认值为0.75
avgFieldLength 这个字段被分词 平均分了多少词
fieldLength 这篇文档这个字段被分词 分了多少词

# others
ES 5.0 之前，默认的相关性算分采用的是 TF-IDF，而之后则默认采用 BM25

tf * idf越大，说明这个词越关键


# question

GET _cat/count/<index>?v 得到的文档数和explain里面看到的docCount对不上？？

weight(title:哪里 in 173) 这个173是怎么知道的

为什么是先max of 后 sum of