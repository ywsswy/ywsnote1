	YPointF p[2]= {YPointF(1, 1) , YPointF(14, 14)};
	YPointF *p1 = new YPointF[4];
这是两种不同类型
第一种都在栈
第二种除了p1指针在0x18
其他的p1[0]。。。都在堆中分配，0x322
【】
new方式为动态内存分配 
Point ptr1=new Point; //1动态创建对象，没有给出参数列表，因此调用默认构造函数
delete ptr1; //删除对象，自动调用析构函数
ptr1=r Point (1 ,2) ;//动态创建对象，并给出参数列表，因此调用有形参的构造函数
delete ptr1; 
point *ptr= new point [2] ; 
delete [] ptr; 
【指针也是有型的。。。。。指针像容器】
f10at * fp;
fp=new float[lO] [25] [10];
便会产生错误。这是因为，在这里 new 操作产生的是指向→个 25 X 10 的二维 float 类型
数组的指针，而 fp 是→个指向 float 型数据的指针。正确的写法应该是:
float (*cp) [25] [10];
cp=new float [10] [25] [10]; 

