## 1)
ssh: connect to host xxx port yyy: Connection timed out
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

解决方案：
一方面确认公钥上传好了，
另一方面就是检查使用的.gitconfig .ssh/id_rsa .ssh/config 三个文件存在并且git clone命令确实使用对了正确位置的这三个文件
第一个文件的生效可以通过快捷方式来验证
第二个文件的生效可以参看work/git/config中的export方式
第三个文件就比较恶心。。有时候不一定是使用的~/.ssh/config，而是有可能是用了其他目录的，具体是哪个目录可以使用ssh-keygen命令来猜测实际目录