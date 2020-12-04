# 服务：mongod
# 实例：mongo

【【安全配置】，首先安装（非服务可以），正常启动实例
sudo mongod --port 27017 --dbpath /home/ubuntu/yfolder/mongodb/data --logpath /home/ubuntu/yfolder/mongodb.log --bind_ip_all --fork
【Connect to the instance
mongo 127.0.0.1:27017
【创建管理员
> use admin
switched to db admin
> db.createUser({
user: "ywsAdmin",
pwd: "123456",
roles:[{ role: "userAdminAnyDatabase", db: "admin" }, "readWriteAnyDatabase" ]
})
【退出连接，杀死服务，以认证模式启动服务
sudo mongod --auth --port 27017 --dbpath /home/ubuntu/yfolder/mongodb/data --logpath /home/ubuntu/yfolder/mongodb.log --bind_ip_all --fork
【【以认证模式登录实例
mongo 127.0.0.1:27017 -u "ywsAdmin" -p "123456" --authenticationDatabase "admin"
【【以后如果想删除的话】】use admin; db.dropUser('ywsAdmin')
【【python中连接】】
import pymongo

mongodbClient = pymongo.MongoClient("localhost",27017)
mongodbClient.admin.authenticate('ywsAdmin','123456')
db = mongodbClient.ytestd
col = db.ywsc

for item in col.find():
    print(item) #除了这里的item是以_id为key的字典类型，其他你都不知道的哟

mongodbClient.close()
'''
col是pymongo.collection类型
有count_documents(filter) delete_many delete_one drop_index find find_one insert_one remove update_one
filter 是查询dict

col.find() 返回的是pymongo.cursor类型
有max count limit min next skip sort where []方法
'''
##collection方法
.find({'question':{'$regex': question}})[0]['right']
##cursor的方法
.sort( '_id',pymongo.DESCENDING) # 默认不写为升序
.find({}).skip(wordskip).limit(1)[0]['word']

