trailing 'shift operator' illegal in base/member initializer list
【因为】
构造函数中，写法非法，仅Data函数与后面的组合之间用分号，其他用逗号。
Date::Date(int y,int m,int d):year(y):month(m):day(d){}
【正确】
Date::Date(int y,int m,int d):year(y),month(m),day(d){}
