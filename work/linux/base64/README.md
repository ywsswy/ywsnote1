加密：
$ echo Hello World | base64
SGVsbG8gV29ybGQK

解密：

$ echo SGVsbG8gV29ybGQK | base64 -d
Hello World