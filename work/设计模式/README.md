# 工厂模式：动态决定new什么类的实例

# 单例模式

# Constructor
# Builder
# 构建器
# 建造者 1
### example
Builder age(int age) { this.age = age; return this; }
Builder tall(int tall) { this.tall = tall; return this; }
使用的时候可以a.age(23).sex(0)...
### 应用：gtest里面的mock方法

# 适配器模式：把原函数再封装一下而已