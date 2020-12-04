1、引用命名空间；</p>
<p>
2、定义一个范围，将在此范围之外释放一个或多个对象。</p>
<p>这里我们说的是第二种情形。</p>
<p>using是可以同时声明多个对象的，例如：</p>
<p>using(DataSet ds1 = new DataSet(),ds2 = new DataSet()){}</p>
<p>在using语句中使用return语句返回值时，using仍然能够保证释放指定的对象。
不但如此，不管是正常程序流结束、使用return语句跳出还是抛出异常，
using范围内的对象都会正常析构。在这一点上有点类似于finally，不过比finally更为及时，
因为无需等待垃圾回收机制启动的时刻，而是在退出using语句时启动！</p>
