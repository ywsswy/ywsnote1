var wtest : Seq[Int] = Seq.empty[Int]
println(wtest.getClass.getTypeName)
wtest = wtest :+ 3
wtest = wtest :+ 4
wtest = 5 +: wtest  // :朝向数组，拼接方式始终都是代码顺序拼接
println(wtest.getClass.getTypeName)


>
wtest: Seq[Int] = List()
scala.collection.immutable.Nil$
wtest: Seq[Int] = List(3)
wtest: Seq[Int] = List(3, 4)
wtest: Seq[Int] = List(5, 3, 4)
scala.collection.immutable.$colon$colon


## sortBy
从小到大排