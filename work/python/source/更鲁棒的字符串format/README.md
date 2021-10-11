```
def YFormat(s, **kwargs):
  import re
  re_str_prefix = '{(?!'
  re_str_suffix = '(?<!'
  link = ''
  for k, _ in kwargs.items():
    re_str_prefix = ''.join([re_str_prefix, link])
    re_str_suffix = ''.join([re_str_suffix, link])
    link = '|'
    re_str_prefix = ''.join([re_str_prefix, k, '}'])
    re_str_suffix = ''.join([re_str_suffix, '{', k])
  re_str_prefix = ''.join([re_str_prefix, ')'])
  re_str_suffix = ''.join([re_str_suffix, ')}'])
  s = re.sub(re_str_prefix, '{{', s)
  s = re.sub(re_str_suffix, '}}', s)
  return s.format(**kwargs)


# 原始字符串中本身就有花括号，此时调用原生的format需要人工处理成两个，YFormat作用就是通过前后视取反断言正则进行替换
s = '{hello}{world}{cmd}{{}{{fakehello}'
s = YFormat(s, hello = 123, world = 456)
print(s)
```