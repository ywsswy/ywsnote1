std::ofstream of1("<file_name");  // default overwrite, std::ofstream::app 追加

    if(of1.is_open()){//check if file is open
        std::cout << "open ok" << '\n';
    }





文件放在一个位置之后

首先，二进制放在哪个位置，无所谓
如果file_path是文件的绝对路径，那肯定ok
如果file_path是相对路径，那么要小心自己【执行命令的位置】（并非二进制所在的位置）；还是那句话，脚本任意放，只需要关心人当前的位置即可

  std::ifstream in(file_path, std::ios::in);