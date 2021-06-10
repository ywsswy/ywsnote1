cohesive 内聚
loosely coupled 松耦合


不用魔数

函数圈复杂度低于10
从1开始，一直往下通过程序
一但遇到以下关键字，或者其它同类的词，就加1：if，while，repeat，for，and，or
给case语句中的每一种情况都加1

衡量指标：C10/KLoC，就是超过10的函数的超出的复杂度之和占千行代码的比例，应该低于6