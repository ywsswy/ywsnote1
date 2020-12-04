启动服务，windows下没有--fork选项
mongod --auth --port 27017 --dbpath /data/db1 --logpath /var/log/mongodb/mongod.log --fork --bind_ip_all

启动实例（连接服务）
mongo 127.0.0.1:27017
mongo 127.0.0.1:<port> -u "ywsAdmin" -p "<password>" --authenticationDatabase "admin"

把运行在那个ip地址的 数据库ywsd备份到C盘下
mongodump.exe -h 127.0.0.1:27017 -d ywsd -o C:\
./mongodump -h 127.0.0.1:10086 -d ymsjsd -o ~/yfolder/proj/mongodb/ -u "ywsAdmin" -p "<password>" --authenticationDatabase "admin"

把C盘ywsd目录里的数据库还原到127.0.0.1:27017的 newywsd数据库中
mongorestore -h 127.0.0.1:27017 -d newywsd C:\ywsd


