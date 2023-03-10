
拿统计词频来说
```
Hello World
Hello Hadoop
Hadoop Goodbye Hadoop
```

1）Map 阶段
将输入文件分成若干个小块，每个小块由一行或多行组成。然后对于每个小块，Map 函数将其处理成一个个键值对，其中键为单词，值为该小块中单词出现的次数
如果上面的文件分为3块，那么每一块可以各自独立（整体并行度为3）的运行计算
结果分别为
```
(Hello, 1) (World, 1)

(Hello, 1) (Hadoop, 1)

(Goodbye, 1) (Hadoop, 2)
```
2）Shuffle 阶段（这是个虚拟概念吗？）
Shuffle 阶段将 Map 阶段输出的键值对按照键进行分组，也就是将所有相同的键归为一组
```
Hello: [(Hello, 1), (Hello, 1)]
World: [(World, 1)]
Hadoop: [(Hadoop, 1), (Hadoop, 2)]
Goodbye: [(Goodbye, 1)]
```
3）Reduce 阶段
最后，在 Reduce 阶段，Reduce 函数对每个键值对进行聚合操作，将每个单词出现的次数加起来，得到最终的结果
```
Hello: 2
World: 1
Hadoop: 3
Goodbye: 1
```