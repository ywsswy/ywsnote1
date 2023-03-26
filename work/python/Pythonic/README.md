《编写高质量代码改善Python程序的91个建议》

命名规范：??
模块小写
类 首字母大写，驼峰
函数 首字母小写，驼峰
变量 全小写，下划线


格式化代码：yapf
yapf -i --style='{based_on_style: pep8, indent_width: 4, column_limit: 120}' <file_name>

格式化检测工具：pip install pylint
pytlint <file_name>
.pylintrc示例：
```
[MESSAGES CONTROL]

# Disable the message, report, category or checker with the given id(s).
disable=all

# Enable the message, report, category or checker with the given id(s).
enable=c-extension-no-member,
       bad-indentation,
       bare-except,
       broad-except,
       dangerous-default-value,
       function-redefined,
       len-as-condition,
       line-too-long,
       misplaced-future,
       missing-final-newline,
       mixed-line-endings,
       multiple-imports,
       multiple-statements,
       singleton-comparison,
       trailing-comma-tuple,
       trailing-newlines,
       trailing-whitespace,
       unexpected-line-ending-format,
       unused-import,
       unused-variable,
       wildcard-import,
       wrong-import-order

[FORMAT]
# Maximum number of characters on a single line.
max-line-length=120

# Maximum number of lines in a module.
max-module-lines=2000
```