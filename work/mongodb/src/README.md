db.ywordandfriend.insertOne({'word':'hello'})
db.ywordandfriend.updateOne({'word':'hello'},{$addToSet:{'friend':{'nihao':'Chinese'}}})



