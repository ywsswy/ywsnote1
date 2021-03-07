准确率(正确率, accuracy)，精确度(precision)， 召回率(recall) 都是计算正条件值 (Condition positive， 正样本).
查准率（Precision）查准率反映了被判定为正例中真正的正例样本的比重
查全率（Recall）查全率反映了被判定的正例占总的正例的比重

准确率(accuracy) = 预测对的/所有 = (TP+TN)/(TP+FN+FP+TN)
精确率(precision) = TP/(TP+FP) = 80%
召回率(recall) = TP/(TP+FN) = 2/3
cqr是query与片段的交集占query的比例， ctr是query和片段的交集占片段的比例

倒排压缩：1，分固定大小的块，一个term的属性（tf，docID）是可以在多个块内的，这样的好处是每个块可以单独解压缩，不用整体解压，高频的term可以缓存；2查分存储，可以节省很多bit位；参考资料：https://www.cnblogs.com/huxiao-tee/p/4644422.html

正排过滤：外部获取的doc_ids，利用引擎内filter的能力过滤掉不可展示的id列表。输入是过滤条件+doc_ids，输出是没被过滤的满足条件的doc_ids
rolling restart：解决内存泄漏，就是如果自己不重启用内存做双buffer切换的话，就是机器大部分时间内存使用率低于50%，但是如果是用重启的方式《斜眼笑》