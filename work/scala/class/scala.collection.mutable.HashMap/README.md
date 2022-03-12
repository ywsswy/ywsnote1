val syncType2Conf = mutable.Map.empty[String, Int]

## contains(key)
map是否包含该key

## 插入/覆盖，类似c++的[] = 
syncType2Conf += (key -> value)

## values
得到key组成的Iterable[]，也是一种HashMap

## toSeq
转成Seq[]，