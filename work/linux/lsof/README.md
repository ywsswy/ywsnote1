## 查看文件占用
lsof <file> # 不知道谁在写这个文件，可以这样看进程id

## 查看端口占用
lsof -i :<port>

# macOS
lsof -i -P -n | grep LISTEN