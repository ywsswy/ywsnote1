#!/usr/bin/env python3

import re
stra = '正则匹配的尝试，很好的实验。'
a = re.findall('的.*[。，]',stra)
print(a)