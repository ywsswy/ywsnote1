pure specifier can only be specified for functions
class Cat{
public:
	Cat(){
		numOfCats++;
	}
	static int getNumOfCats(){
		return numOfCats;
	}
private:
	static int numOfCats = 0;//这里不能赋值
};
初始化拿到外面来
int Cat::numOfCats = 0;
