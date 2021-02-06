#include <iostream>


//[捕获列表](参数列表)->返回类型{函数体}
int main() {
	auto Add = [](int a, int b)->int {
		return a + b;
	};
	std::cout << Add(1, 2) << std::endl;
	return 0;
}


//编译器可以自动推断出lambda表达式的返回类型，所以我们可以不指定返回类型
//[捕获列表](参数列表){函数体}


Lambda表达式是通过生成一个小class实现的，这个类重载了operater（）函数，所以它的行为非常像一个函数。Lambda函数就是这个class的实例；当这个类构造时，上下文中的任何变量都可以传进Lambda function class，并作为成员变量保存起来。这事实上这有点像一个已经存在的functor（仿函数）。这样就解决了我的问题，Capture用于传递上下文的环境变量，在class的构造函数中使用；Parameter list是operator（）函数的参数列表