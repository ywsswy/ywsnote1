##  pragma omp parallel for  是OpenMP中的一个指令，用于并行化for循环

编译器会根据指定的并行度自动将for循环分割成多个子任务，并在多个线程上并行执行这些子任务，使用形式为：

```
#include <iostream>
#include "omp.h"
using namespace std;
int main(int argc, char **argv) {
	omp_set_num_threads(4);  //设置线程数，一般设置的线程数不超过CPU核心数，这里开4个线程执行并行代码段
#pragma omp parallel for
	for (int i = 0; i < 6; i++)
		printf("i = %d, I am Thread %d\n", i, omp_get_thread_num());
        }
}
```

运行结果形如：（四个线程并发负责某个for迭代）

```
i = 4, I am Thread 2
i = 2, I am Thread 1
i = 0, I am Thread 0
i = 1, I am Thread 0
i = 3, I am Thread 1
i = 5, I am Thread 3
```

## pragma omp parallel for schedule(dynamic) 等价于 pragma omp parallel for schedule(dynamic, 1)

迭代任务会动态地决定分配给哪个线程，谁先执行完就分配给谁，每次分配1个