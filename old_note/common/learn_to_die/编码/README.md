printf "%x" "'春"
6625
# unicode 0x6625（0110 0110 0010 0101）
echo -n "春" |hexdump -C
00000000  e6 98 a5                                |......|
# utf-8 E6 98 A5
# url_encode %E6%98%A5
|Unicode|utf-8|
|---|---|
|0x0 ~ 0x7F|0XXXXXXX|
|0x80 ~ 0x7FF|110XXXXX 10XXXXXX|
|0x800 ~ 0xFFFF|1110XXXX 10XXXXXX 10XXXXXX|
|0x10000 ~0x1FFFF|11110XXX 10XXXXXX 10XXXXXX 10XXXXXX|

0x6625 属于 0x800 ~ 0xFFFF，所以utf-8为
1110     10       10
    0110   011000   100101
11100110 10011000 10100101
E6 98 A5




