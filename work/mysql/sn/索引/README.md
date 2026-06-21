内部的数据结构，用于加速数据库查询操作


常见索引类型（在InnoDB-mysql最新版本的默认存储引擎中，所有的索引都用B+树实现）
- 主键索引 (PRIMARY KEY) （建表时关键字，自动创建索引）唯一标识每行，不允许 NULL，每表只能有一个
- 唯一索引 (UNIQUE)	（建表时关键字，自动创建索引）列值唯一，允许 NULL
- 普通索引 (INDEX)（需要手动建表时指定，当然也支持建表后原地加索引-create index/alter table）最基本的索引，无唯一性限制，指定示例：
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    INDEX idx_name (name)           -- 手动指定普通索引
);


mysql支持EXPLAIN + SQL命令，不会真正执行完整结果（不会操作增删改），而是探测一下执行计划（告诉你数据库打算怎么执行这条 SQL）
输出几个信息：
- rows 预计扫描多少行，越大说明越慢
- key：具体使用了某个索引，null表示没使用索引
- type（查询连接类型）：
-- const / eq_ref → 很快，例如是用主键或唯一索引等值匹配
-- ref → 走了普通索引
-- range → 走了范围扫描索引
-- ALL → 没走索引，是全表扫描，最慢，需要优化



## 其他
- SHOW VARIABLES LIKE 'default_storage_engine'  -- 查看存储引擎
- SHOW Index from t  -- 查看表建立哪几个索引，以及底层数据结构实现

- 记录一个有趣的索引优化：

select * from t where server_id = 333

性能比下面的差：

select * from t where server_id = "333"

通过EXPLAIN命令就能看出来，前者是全表扫描（应该是要操作字符串隐式转换），后者是使用const索引