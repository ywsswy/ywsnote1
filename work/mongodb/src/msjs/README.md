//db.ywsc.find({right:{$exists:false}})

//db.ywsc.find({question:{$regex:"不算装备,联盟科技有几"}}).sort({_id:-1})
//db.ywsc.deleteOne({question:{$regex:"不算装备,联盟科技有几"}})

var str = "武将计谋成功率跟什么有关"
var str_w = ""
var str_r = ""
//db.ywsc.find({question:{$regex:str}}).sort({_id:-1})
var res = db.ywsc.find({question:{$regex:str}}).sort({_id:-1})
if(str_w.length == 0 && str_r.length == 0){
    if(res.size() == 0){
        print("no question,insert")
        db.ywsc.insert({question:str})
    } else{
        print("exist: "+res.size().toString())
        db.ywsc.find({question:{$regex:str}}).sort({_id:-1})
    }
}
else{
    print('update')
    if(str_w.length > 0){
        db.ywsc.updateOne({question:{$regex:str}},{$addToSet: {wrong:str_w}})
    }
    else{
        db.ywsc.updateOne({question:{$regex:str}},{$set: {right:str_r}})
    }
}
