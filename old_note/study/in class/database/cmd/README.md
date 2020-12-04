	删除			修改
数据	delete（from where）	update（set）



# replace into 可以支持，有则更新，无则插入
REPLACE INTO `<库名>`.`<表名>` (
	`<字段名1>`,
	`<字段名2>`
)
VALUES
	(
		'4',
		'{\"value\":[\"value\"]}'
	);



# 建表DDL
CREATE TABLE `<表名>` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增id',
  `<字段名2>` tinyint(1) unsigned DEFAULT '0' COMMENT '这里是注释',
  `<字段名3>` varchar(1024) DEFAULT '',
  `<字段名4>` text,
  `<字段名5>` bigint(20) DEFAULT '0' COMMENT '注意这里虽然是数值类型，但是也能看到单引号',
  `<字段名6>` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `<字段名2>` (`status`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8mb4 COMMENT='这里也可以写关于表的注释';



