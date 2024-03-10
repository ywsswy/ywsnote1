三元运算符的结合律
	/*
	double m = 1.0;
	double t = 4.4;
	m == 1 ? t = 5.5 : t = 6.6;
	m == 1 ? (cout << "can post") : (cout << "can not post");
	m == 1 ? t = 5.5 : (cout << "can post");
	/*
	duanlu P1 ? Z1 : P2 ? Z2 : Z3  
	a == -1 ? cout << "Z1" : a == -1 ? cout << "Z2" : cout << "Z3";
	P1 true : Z1
	P1 false P2 true : Z2
	P1 false P2 false : Z3
	P1 ? P2 ? Z1 : Z2 : Z3
	a == -2 ? a == -1 ? cout << "Z1" : cout << "Z2" : cout << "Z3";
	P1 f : Z3
	P1 t P2 t : Z1
	P1 t P2 f : Z2
	sanceng
	*/
