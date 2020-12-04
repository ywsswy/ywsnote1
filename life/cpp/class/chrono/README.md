#include <chrono>

std::chrono::time_point<std::chrono::system_clock> start = std::chrono::system_clock::now();

// do something

std::chrono::time_point<std::chrono::system_clock> end = std::chrono::system_clock::now();
std::chrono::duration<double> diff = end-start;
std::cout << diff.count() << " s\n";