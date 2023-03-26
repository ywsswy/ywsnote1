https://stackoverflow.com/questions/33050620/golang-style-defer-in-c#include <memory>
```
#include <iostream>

using defer = std::shared_ptr<void>;

int main() {
    defer _(nullptr, [](...){ std::cout << " !"; });
    defer __(nullptr, [](...){ std::cout << " World"; });
    std::cout << "Hello";
}
```