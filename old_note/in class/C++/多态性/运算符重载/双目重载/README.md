class YComplex{
public:
	YComplex(double real = 0,double imag = 0):real(real),imag(imag){}
	friend YComplex operator+(const YComplex &c1,const YComplex &c2);
	friend YComplex operator-(const YComplex &c1,const YComplex &c2);
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
YComplex operator+(const YComplex &c1,const YComplex &c2){
	return YComplex(c1.real+c2.real,c1.imag+c2.imag);
}
YComplex operator-(const YComplex &c1,const YComplex &c2){
	return YComplex(c1.real-c2.real,c1.imag-c2.imag);
}
//////////////////////////////////////////////////////////////////////
	YComplex c1[2] = {YComplex(1,2),YComplex(3,4)};
	c1[1] = c1[0]+c1[0];
	c1[1].show();
	(5.0+c1[1]).show();
	(c1[1]-5.0).show();
