## 1. 普通写入
std::ofstream of1("of1");  // default overwrite, std::ofstream::app 追加 of1; of1.open("of1");

if(!of1.is_open()){  //check if file is open
  std::cout << "open fail" << std::endl;
}

## 2. 二进制写入
std::ofstream of1("of1", std::ios::binary);

std::string s = "hello";
if(!of1.is_open()){
  std::cout << "open fail" << std::endl;
}
of1.write(s.c_str(), s.size());
uint64_t id = 1;
of1.write((char*)&id, 8); // 其他类型的写入

## 3. 普通读取（很少用这个，一般是其他API依赖instream时才用到）
std::ifstream in(file_path, std::ios::in); // |std::ios::binary

## 4. 二进制读取
std::ifstream if1("of1", std::ios::binary);
if (!if1.is_open()) {
  std::cout << "open fail" << std::endl;
}
std::string s(std::istreambuf_iterator<char>{if1}, std::istreambuf_iterator<char>{});
或者mmap

## 其他：
### 文件路径说明

首先，二进制放在哪个位置，无所谓
如果file_path是文件的绝对路径，那肯定ok
如果file_path是相对路径，那么要小心自己【执行命令的位置】（并非二进制所在的位置）；还是那句话，脚本任意放，只需要关心人当前的位置即可

### 获取文件大小
https://blog.csdn.net/u010261063/article/details/108080002