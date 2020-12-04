class Cat{
public:
	Cat(){
		numOfCats++;
	}
	static int getNumOfCats(){
		return numOfCats;
	}
private:
	static int numOfCats;
};
Cat::numOfCats = 0;//光有名字还不行，还需要声明类型
int Cat::numOfCats = 0;
