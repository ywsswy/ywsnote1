首先A项目有一个git仓库
然后B项目需要依赖于这A
则在B中，指定某个文件夹下放A模块
git submodule add <git address of A> <path in B>


别人使用的时候
先clone B，然后在B中
git submodule sync  # 如果B项目在commit1上，git submodule add rX pB，然后在commit2上，git submodule add rY pB，此时从commit1切换到commit2上，不能仅仅git reset --hard commit2，还要git submodule sync强制更新一下submodule，就是说同名（pB）的切换不够智能
git submodule update --init --recursive  # 这个可以找到B当时依赖的A的commit，并不随A仓库的更新而更新，更新只能在B仓库中发起


# 删除

git submodule deinit <sub>
git rm <sub> #这个就能移除.gitmodule里面的内容了

???删除submodule可能删不干净，方法是要删掉后git rm --cached xxx


#
git submodule summary/status  # 不靠谱的命令

