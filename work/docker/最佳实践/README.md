结论：
工作目录公用，不希望在docker内被限制，所以直接就挂-v /root/:/root/
[全都是root，不要用普通用户]，因为1）docker内如果是root用户，没有权限访问母机的$HOME/.ssh，拉代码失败；2）docker内如果是root用户，docker内保存的文件，母机普通用户反而无法读写
docker run --network=host -it / -v /data/.cache/:/data/.cache/ -v /data/workspace/:/data/workspace/ --name search-trpc-cpp-compile_015_v2 --security-opt seccomp=unconfined 6677ece0205e
history |grep docker |grep search-trpc-cpp-compile_015_v2


# 改变想法！自己拥有root用户