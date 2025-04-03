Redis协议是一种简单的文本协议，称为RESP（REdis Serialization Protocol）。当你使用 telnet 或其他 TCP 客户端直接与 Redis 进行通信时，返回的数据格式可以通过 RESP 协议进行解析。以下是 RESP 协议的主要数据类型及其格式：

简单字符串（Simple Strings）：
以 + 开头。
例如：+OK\r\n 表示一个简单字符串 "OK"。


错误（Errors）：
以 - 开头。
例如：-Error message\r\n 表示一个错误信息。

整数（Integers）：
以 : 开头。
例如：:1000\r\n 表示整数 1000。

批量字符串（Bulk Strings）：
以 $ 开头，后跟字符串长度。
例如：$6\r\nfoobar\r\n 表示字符串 "foobar"。
如果字符串长度为 -1，表示空值（nil），如：$-1\r\n。

数组（Arrays）：
以 * 开头，后跟元素数量。
例如：*2\r\n$3\r\nfoo\r\n$3\r\nbar\r\n 表示一个包含两个批量字符串的数组 ["foo", "bar"]。
如果元素数量为 -1，表示空数组，如：*-1\r\n。