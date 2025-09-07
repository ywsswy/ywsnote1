#include <unistd>

    pid_t pid = fork();
    if(pid == -1) //失败
    {
        return;
    }
    if(pid == 0) { // 子进程执行的语句块
    {
        
    } else { // 父进程执行的语句块

    }

// 父子进程使用管道通信
#include <iostream>
#include <vector>
#include <string>
#include <unistd.h> // fork, pipe
#include <sstream>  // std::ostringstream, std::istringstream

// 序列化 std::vector<std::string> 为一个字符串
std::string serializeVector(const std::vector<std::string>& vec) {
    std::ostringstream oss;
    for (size_t i = 0; i < vec.size(); ++i) {
        oss << vec[i];
        if (i != vec.size() - 1) {
            oss << '\n'; // 使用换行符作为分隔符
        }
    }
    return oss.str();
}

// 反序列化字符串为 std::vector<std::string>
std::vector<std::string> deserializeVector(const std::string& str) {
    std::vector<std::string> vec;
    std::istringstream iss(str);
    std::string line;
    while (std::getline(iss, line)) {
        vec.push_back(line);
    }
    return vec;
}

int main() {
    int pipefd[2];
    if (pipe(pipefd) == -1) {
        perror("pipe");
        return 1;
    }

    pid_t pid = fork();
    if (pid == -1) {
        perror("fork");
        return 1;
    }

    if (pid == 0) {
        // 子进程
        close(pipefd[0]); // 关闭读端

        // 创建一个 std::vector<std::string>
        std::vector<std::string> vec = {"Hello", "World", "From", "Child"};

        // 序列化为字符串
        std::string serialized = serializeVector(vec);

        // 写入管道
        write(pipefd[1], serialized.c_str(), serialized.size() + 1); // 包括终止符
        close(pipefd[1]); // 关闭写端
    } else {
        // 父进程
        close(pipefd[1]); // 关闭写端

        char buffer[1024];
        ssize_t bytes_read = read(pipefd[0], buffer, sizeof(buffer));
        close(pipefd[0]); // 关闭读端

        if (bytes_read > 0) {
            // 反序列化为 std::vector<std::string>
            std::vector<std::string> vec = deserializeVector(buffer);

            // 输出结果
            std::cout << "Received vector from child process:" << std::endl;
            for (const auto& str : vec) {
                std::cout << str << std::endl;
            }
        } else {
            std::cerr << "Failed to read from pipe." << std::endl;
        }
    }

    return 0;
}