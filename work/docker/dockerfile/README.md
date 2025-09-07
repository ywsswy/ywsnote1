docker build . -t host.com/a/b:test

用当前目录的dockerfile打镜像

如果想看过程的详细输出（例如RUN sh a.sh）
方法应该是在docker 命令后面重定向，而不应该是RUN sh命令后面重定向;