【上传到github的方法】
github create a repository(empty, don't add any file)
workfolder: git init&add&commit
git remote add origin git@github.com:ywsswy/wannasee.git
把自己的公钥文件添加到https://github.com/settings/keys里面，推荐ip/machine_user，（每个key对应一个fingerprint，可以使用ssh-keygen -E md5 -lf id_rsa.pub看对应的哪个）
ssh -T git@github.com可以测试是否成功）
git push -u origin master # 第一次push
以后只需要git push