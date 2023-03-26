class YComplex{
public:
	YComplex(double real = 0,double imag = 0):real(real),imag(imag){}
	friend YComplex operator+(const YComplex &c1,const YComplex &c2);
	friend YComplex operator-(const YComplex &c1,const YComplex &c2);
	friend ostream &operator<<(ostream &out,const YComplex &c);
	void add(YComplex c){
		this->real = this->real+c.real;
		this->imag = this->imag+c.imag;
	}
private:
	double real;
	double imag;
};
ostream &operator<<(ostream &out,const YComplex &c){
	out<<c.real<<"+"<<c.imag<<"i";
	return out;
}
	YComplex c1[2] = {YComplex(1,2),YComplex(3,4)};
	c1[1] = c1[0]+c1[0];
	cout<<c1[1]<<endl;
	cout<<5.0+c1[1]<<endl;
	cout<<c1[1]-5.0<<endl;
