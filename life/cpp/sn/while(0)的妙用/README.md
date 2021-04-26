#define FOO(x) do {xxx;xxx} while(0)


// 这种写法是针对有的时候define了两句话，然后if不用花括号包起来，然后单行调用的FOO，会导致后面的else无法匹配

  if (true)
    FOO(wolrd);
  else //如果不用上面的do{}while(0)包住，这里一定会报错，因为展开后 if (true) xxx;xxx;else 非法
    std::cout << "yyy";