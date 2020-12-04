# 注意事项
- /hits/total 是搜索到的文档数，而不是最终显示的数量
只有sort、must、should是数组，且数组中不会紧跟着数组，而是一个个object
父子关系：
祖宗：from、size、query、sort、
query的儿子可以是bool
bool的儿子是可以是should、must、filter
match、term的儿子一定是字段名
match的孙子可以是query、boost
term没有孙子
filter的儿子可以是match