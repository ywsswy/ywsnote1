			MongoDB实例
	database  	database数据库
	table	  	collection集合
	row		document	文档
	属性列	  	field		字段（key_string-value_any)
			index
			primary key(MongoDB自动将_id字段设置为主键)

any(string,int,float,timestamp,binary,document,数组//数据类型：http://www.runoob.com/mongodb/mongodb-databases-documents-collections.html
集合没有固定的结构，这意味着你在对集合可以插入不同格式和类型的数据


内嵌了javascript所以能用typedof var a = new Data()等
ObjectId:"5bd659fab3e34d658c7e971b",前四个字节是时间戳，0x5bd659fa .getTimeStamp()
String utf-8
Date
Timestamp
Int32

要指定类型

https://docs.mongodb.com/manual/reference/operator/query/expr/

http://www.runoob.com/python3/python-mongodb.html