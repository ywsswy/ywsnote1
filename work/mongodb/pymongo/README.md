#############################python 连接mongodb
import pymongo
client = pymongo.MongoClient("<hostname>",<port>)
client.admin.authenticate('ywsAdmin','<password'>)
clientDb = client.ywannaseed		#数据库
col = clientDb['ymonitorc']	#表

sql = col.find(projection={'_id': False, 'site': True, 'status': True})#
for tempUrl in sql:
	print(temp) #除了这里的item是以_id为key的字典类型，其他你都不知道的哟
mongodbClient.close()

###############################type
'_id':bson.ObjectId(str_id)
##################################################################################3
col		是pymongo.collection类型
col.find() 	返回的是pymongo.cursor类型
###collection方法
.find(filter={'question':{'$regex':question,'$options':'i'}})[0]['right'] #i忽略大小写
.find(filter,projection)
.update_one(filter={'_id':tempUrl['_id']},update={'$set':{'status':'0'}})
.update_one()(filter,update)
有count_documents(filter) delete_many delete_one drop_index find find_one insert_one remove
filter 是查询的dict
projection通过设置字段true or false 控制返回哪些字段，除了id为False时写，其他的只需要写要显示的True
update是操作符dict,操作符有set设置，addToSet添加到集合，inc增加
###cursor的方法
有max count limit min next skip sort where []方法
.sort( '_id',pymongo.DESCENDING) # 默认不写为升序
.find({}).skip(wordskip).limit(1)[0]['word']
顺序应该是find > sort > skip > limit，即使你写的时候顺序不一样，但是mongodb内部是强制按这个顺序的


