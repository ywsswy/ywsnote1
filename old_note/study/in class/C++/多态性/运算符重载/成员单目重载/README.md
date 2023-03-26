class YClock{
public:
	YClock(int newH = 0,int newM = 0,int newS = 0):hour(newH),minute(newM),second(newS){}
	//!should set to now.
	YClock & operator++(){//前置单目，本身修改，并返回修改后的自身引用
		second++;
		if(second>=60){
			second = 0;
			minute++;
			if(minute>=60){
				minute = 0;
				hour=(hour+1)%24;
			}
		}
		return *this;
	}
	YClock operator++(int){//后置单目，先拷贝成复制本，然后修改本身，最后返回复制本
		YClock old = *this;
		++(*this);
		return old;
	}
	void showTime(){
		cout<<hour<<":"<<minute<<":"<<second<<endl;
	}
private:
	int hour,minute,second;
};
///
	YClock c1[2];
	c1[0].showTime();
	(++c1[0]).showTime();//等价于：c1[0].operator++().showTime();
	c1[0].showTime();
	(c1[0]++).showTime();//等价于：c1[0].operator++(0).showTime();
	c1[0].showTime();

