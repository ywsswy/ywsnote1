One vs Rest
n次分类
n个类别中，选n-1个类合并当作同一类，然后跟剩余的一个类训练一个二分类器，然后加入新样本点，看样本点属于哪一类，计算此时得分。
然后重新选下一个n-1类。。。。
最后看哪个得分最高，用此划分方法来判断新样本属于哪一类
One vs One
C(n,2)次分类
选2个类，不看其他无关类