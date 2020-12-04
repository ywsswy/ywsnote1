##时区转换
import pymongo
import datetime


i = col.find_one()
print(i)
j = i['time'].replace(tzinfo=datetime.timezone.utc).astimezone(tz=None)
print(j)
d = datetime.timedelta(minutes=5)
k = j+d
print(k)
col.insert_one({'time':k})
