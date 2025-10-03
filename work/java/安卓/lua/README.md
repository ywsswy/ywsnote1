'.' 操作符：
普通的函数调用
不会自动传入self参数
通常用于静态方法或工具函数

- 独立的函数的写法

```
-- （两个短线是注释）
local A = {}
function A.f1(a)
    -- 函数实现，无self
end
-- 别的文件调用时
local A = require("A")  -- 双引号和单引号是等价的
A.f1(xx)  -- 行尾分号可省略

```

- 面向对象的写法

```
local B = class("B")
-- 注意跟A的区别，这里是冒号
function B:f1(a)  -- 等价于function B.f1(self, a)
    -- 函数实现，有self
    self.xxx = yyy
end
-- 别的文件调用时
local B = require("B")
B:f1(xx)  -- 等价于B.f1(B, xx)
```

遍历数组的写法（Lua中，数组本质上也是table，只是使用了连续的数字作为键。所以pairs可以遍历所有类型的table，包括数组。）

```
for i, v in pairs(array) do  -- 假如是纯数组（连续索引）[10,55,23...]
    -- i是索引是1,2,3...，v是数组的每个值10,55,23...
end

-- 针对连续索引可以使用#array来输出size
```


device.writablePath  -- 等于 /data/data/<app>/files/ 目录