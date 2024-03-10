传递实参，是先复制后面的参数
类的组合：
构造先从内嵌对象开始构造，然后构造本类。
内嵌对象的构造顺序与初始化列表中出现的顺序无关，
只取决于类定义时定义的先后顺序。（Point p1,p2;p1是在p2前的）
显示调用构造函数和析构函数就像调用一般的函数一样, 并不意味着创建或销毁对象; 
析构函数中的return才是开始释放响应内存的，之前的都是人为设定的一些处理
【基本数据类型】
bool\char\short\int\long\float\double\long double
int a1;//基本数据类型，不分配时默认为-858993460=(0xcc 0xcc 0xcc 0xcc)
float f1;//同上，-1.0737418e+008=(0xcc 0xcc 0xcc 0xcc)
char c1;//同上，-52=（0xcc）
char s2[20];//同上，烫烫烫烫烫烫烫烫烫烫=（0xcc 0xcc 0xcc...）
【1】
Line(Point &p1,Point &p2);
Line(Point p1,Point p2);//与前者相比，后者会调用复制构造函数（传参数为传值调用）
【2】
Line::Line(Point &p1,Point &p2)/*:p1(p1),p2(p2)*/{
		this->p1 = p1;
//不会调用复制构造函数，就是普通赋值而已，赋值运算符(重载)在起作用，单纯的复制字段
//因为复制构造函数只会在创建对象的时候调用
		this->p2 = p2;
		double x = static_cast<double>(this->p1.getX()-this->p2.getX());
		double y = static_cast<double>(this->p1.getY()-this->p2.getY());
		this->len = sqrt(x*x+y*y);
		log1.w(YFL,Log::TRACE,"Line::Line(Point p1,Point p2):p1(p1),p2(p2){\
			\t\tthis=%#p,tp1.x=%d,p1.y=%d,p2.x=%d,p2.y=%d",
			this,this->p1.getX(),this->p1.getY(),this->p2.getX(),this->p2.getY());
	}
	Line::Line(Point &p1,Point &p2):p1(p1),p2(p2){
		double x = static_cast<double>(this->p1.getX()-this->p2.getX());
		double y = static_cast<double>(this->p1.getY()-this->p2.getY());
		this->len = sqrt(x*x+y*y);
		log1.w(YFL,Log::TRACE,"Line::Line(Point p1,Point p2):p1(p1),p2(p2){\
			\t\tthis=%#p,tp1.x=%d,p1.y=%d,p2.x=%d,p2.y=%d",
			this,this->p1.getX(),this->p1.getY(),this->p2.getX(),this->p2.getY());
	}
//与前者相比，后者调用了Point的复制构造函数，创建对象的时候就做好了工作
//前者创建类时调用了Point的普通构造函数，此时没有做好工作，后面的this->p1 = p1又进行了简单赋值
【3】
Line(const Point &p1=Point(7,8),const Point &p2=Point(9,10));//声明
Line::Line(const Point &p1,const Point &p2):p1(p1),p2(p2)//定义
【4】
	Clock c1 = Clock(0,0,0,0);
	Clock c2(0,0,0,0);
	Clock c3;
//这三种方式一模一样

