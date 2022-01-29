# map  // 对每个元素进行处理得到下一个结果

val shardNo2PartitionNo = 0.until(8).map(shardNo => (shardNo, shardNo % 2))


# reduce // 依次对每个元素规约

(1 to 9).reduceLeft( _ * _) //相当于1 * 2 * 3 * 4 * 5 * 6 * 7 * 8 * 9 
(1 to 9).reduceLeft( _ + _) //相当于1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 
(1 to 9).reduce(_ + _) //默认是reduceLeft

