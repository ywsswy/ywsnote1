######################################################################################基础操作
use yws_database;//创建/切换数据库
show dbs;
db		//显示当前数据库
db.createCollection('yws_collection')
show collections //显示当前数据库里的表
db.<collection_name>.drop() //删除集合/表

operator:&+ exists regex set
$gt表示>，$lt表示<,$gte表示>=，$lte表示<=,$eq表示==，$ne表示!=
$in		{ field: { $in: [<value1>, <value2>, ... <valueN> ] } }
$nin
keyword: true false (不能大写首字母True False)

******************************************************************************************
db.products.find( { qty: { $gt: 25 } }, { item: 1, qty: 1 } ).pretty() #第一个参数的query，第二个是projection投影（返回哪些列），_id默认为1，其他默认为0，pretty美化输出
对find结果操作：
.projection({})
.sort({ _id: -1 }) //以_id 排序，-1是降序，默认1是升序
.skip(1) //跳过前面多少条实现分页
.limit(4)
db.ywordc.deleteMany({stage:{$exists:false}}
db.ymonitorc.updateOne({site:'http://www.baidu.comnima'},{$set:{status:1}) #({条件},{$set:{更新内容}})  //1是Double的1
db.ywsc.insert({word:'new2',stage:NumberInt(5),time:new Date()}) #文档在{}中，key不用加引号，value如果是String要加引号(单引号双引号一样），会默认加入ObjectId类的_id field属性列, String
db.ywsc.deleteOne({_id:{$lt:db.ywsc.find().sort({'_id':-1})[0]['_id']}}) #deleteMany
db.ywsc.find({question:{$regex:"明天"}}).sort({_id:-1}) #正则
#往数组中插入
db.ywsc.updateOne({question:{$regex:/abc/i}},{$addToSet: {wrong:'重庆'}}) #/object/<option> i代表忽略大小写

{ "_id" : ObjectId("5bd659fab3e34d658c7e971b"), "word" : "ability" } //

> db.ywsC.find({qty:{$in:[5,15]}})
{ "_id" : ObjectId("5bcec10ca5c4eeefd27f718c"), "item" : "a", "qty" : 5 }
{ "_id" : ObjectId("5bcec1bea5c4eeefd27f718d"), "item" : "a", "qty" : 5 }
{ "_id" : ObjectId("5bcec1e9a5c4eeefd27f7190"), "item" : "ab", "qty" : 15 }
> 

查询item以a或b开头的数据（可以用正则表达式）
db.collection.find( { item: { $in: [ /^a/, /^b/ ] } } )


> var t1 = db.ywsc.find()
> t1.toArray(function(err,result){console.log(result);})

聚合统计，比如计算每个作者分别发了多少文章
# http://www.runoob.com/mongodb/mongodb-aggregate.html
db.ychatc.aggregate([{$group:{_id: "$dev", times:{$sum:1}}},{$sort:{times:-1}}])#每个设备发言次数
