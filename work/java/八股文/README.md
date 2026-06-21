```
import java.util.Scanner;
import java.util.*;
public class Main {
    public static int f(int[] prices) {
      // ...
    }
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int[] prices = Arrays.stream(sc.nextLine().trim().split("\\s+"))
                           .mapToInt(Integer::parseInt)
                           .toArray();
        System.out.println(f(prices));
    }
}
```

🟢 基础层次
1. 数据类型与基础语法
Q：Java 中基本数据类型有哪些？int 和 Integer 的区别？

8种基本类型：byte、short、int、long、float、double、char、boolean

int 是基本类型，存在栈上；Integer 是包装类，存在堆上（由GC决定何时回收）

Integer 有自动装箱/拆箱机制；Integer 缓存了 -128~127 的值（== 比较时注意）

Q：== 和 equals() 的区别？

== 比较的是引用地址（基本类型比较值）

equals() 默认也是比较地址，但 String、Integer 等类重写了它，比较的是内容

Integer.valueOf() 内部有缓存池，-128~127 范围内返回同一个对象，超出范围则 new 新对象。

Integer a = 127;
Integer b = 127;
System.out.println(a == b);   // ✅ true（同一个缓存对象）

Integer c = 128;
Integer d = 128;
System.out.println(c == d);   // ❌ false（两个不同的新对象）

结论：Integer 之间比较永远用 equals()，不要用 ==，除非你明确知道值在缓存范围内。

Q：String、StringBuilder、StringBuffer 的区别？

String：不可变，每次操作产生新对象

StringBuilder：可变，线程不安全，性能高

StringBuffer：可变，线程安全（方法加了 synchronized），性能略低


3. 集合框架
Q：HashMap 的基本原理？

底层是 数组 + 链表 + （Java 8又引入了红黑树）

通过 key.hashCode() 计算下标，冲突时用链表存储

链表长度 ≥ 8 且数组长度 ≥ 64 时，链表转为红黑树

默认初始容量 16，负载因子 0.75，扩容时 2 倍扩容

Q：HashMap 和 HashTable 的区别？

HashTable 线程安全（方法加 synchronized），HashMap 不安全

HashMap 允许 key/value 为 null，HashTable 不允许

现在线程安全场景推荐用 ConcurrentHashMap

🔴 进阶层次
1. 多线程与并发

Q：volatile 关键字的作用？

可见性：修改后立即刷新到主内存，其他线程立即可见

禁止指令重排序：通过内存屏障实现

不保证原子性：i++ 这类操作仍然线程不安全

2. JVM
Q：GC 的常见算法？新生代/老年代用什么收集器？

标记-清除：产生碎片

标记-整理：无碎片，适合老年代

复制算法：无碎片，适合新生代（Eden:S0:S1 = 8:1:1）

常见收集器：新生代用 ParNew，老年代用 CMS；或统一用 G1（推荐）
