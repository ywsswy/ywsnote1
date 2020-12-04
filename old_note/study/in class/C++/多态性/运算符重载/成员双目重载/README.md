class YComplex{
public:
	YComplex(double real = 0,double imag = 0):real(real),imag(imag){}
	YComplex operator+(const YComplex &c2)const{
		return YComplex(this->real+c2.real,this->imag+c2.imag);
	}
	YComplex operator-(const YComplex &c2)const{
		return YComplex(this->real-c2.real,this->imag-c2.imag);
	}
	void add(YComplex c){
		this->real = this->real+c.real;
		this->imag = this->imag+c.imag;
	}
	void show(){
		cout<<real<<"+"<<imag<<"i"<<endl;
	}
private:
	double real;
	double imag;
};
//////////////////////////////////////////////////////////////////////
	YComplex c1[2] = {YComplex(1,2),YComplex(3,4)};
	c1[1] = c1[0]+c1[0];//等价于：c1[1] = c1[0].operator +(c1[0]);
	c1[1].show();
	

