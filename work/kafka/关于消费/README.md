topic里面的一个partition有最新offset/生产offset和最旧offset(0s)，最新的只会递增，跟消费者无关

每一个消费组里有关于该topic下某partition的的提交offset和最后提交时间，提交offset <= 最新offset，提交offset可以被消费者修改，