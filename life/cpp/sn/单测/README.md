

本来，是原地build（生成gcno），原地运行（生成gcda）
但如果希望把二进制拷贝到【另外机器】上运行得到覆盖率，则需要设置
export GCOV_PREFIX="/home/hill/gcda/"
export GCOV_PREFIX_STRIP=8 #这个表示原来原地生成的gcda路径中前多少层目录替换成$GCOV_PREFIX

然后运行完把gcda拷贝回来做统计