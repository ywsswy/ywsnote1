[git代码管理工具学习](https://git-scm.com/book/zh/v2)
```
commit message规范，必选的
<type>: <subject>
type种类：feat(merge) fix | build style revert docs refactor | test perf
ps: 需求开发层级：epic（史诗级大项目，非必选）、feature（可独立发布上线的需求，必须拆分成至少1个story）、stroy（细粒度）
subject格式：简短描述 --关联标识=<id>

git branch -D <local_branch> #删除本地分支
git branch -m '重命名分支名'
git checkout <branch_name> #切换分支。一是使 HEAD 指回 branch_name分支，二是将工作目录恢复成 branch_name分支所指向的快照内容
git checkout [<other branch_name>] -- <file>#把那个分支的文件覆盖过来 666，省略分支则是表示HEAD
git cherry-pick <commit> #把其他分支的某次修改弄到当前来; git cherry-pick <start(old)>^..<end>把一个区间的修改弄过来，加了^符号，就是左闭右闭
git clean -f
git commit --amend #修改commit信息
git diff <old commited/branch> <new commited/new brand> [<file_name>]#注意此刻的untracked file不会参与到diff中
git diff # 常规diff（当前与暂存区比较）
git diff HEAD # 如果有暂存区等影响如merge stash_pop等，则用这个，或者发现git diff异常的时候，stash_save_then_pop就发现又好了
  参数--filter=M 仅显示更改的文件，不知道新增的文件
  参数--name-status 不管详细内容，只管文件是A/D/M
git fetch -p 获取所有远程分支改动信息（p可以保证删掉的远程分支不再显示）（如果某个分支显示 xxxxxx...yyyyyyy br1  (forced update) 表明有人force到yyyyyyy，xxxxx没有参考意义！）
git log # 查看历次详细commit eg:commit 56933675c782a232668c6fcbfffd249625c93947(版本号)
git log --graph --decorate --oneline --all #这个是最详细(--simplify-by-decoration只看分支粒度的改动)
git ls-files #查看仓库了有哪些文件
git merge #最粗暴的merge，就是两个树枝二合一；--squash 是把来源分支上的所有修改都压缩成一次修改，这样就可以只提交一个commit到当前分支，来源树枝不动，好处是当前分支的生长非常干净，但是这种是改变了原作者的commit信息/时间（rebase的方法可以保留）
git mergetool #git merge出现冲突的时候使用； diffg L/R可以选择采用本地/远程的修改，]/[c可以跳到下/上一处修改；我建议mergetool完之后进行一次全搜<======，<<<<<<<，=======，>>>>>>>
git push origin <remote_branch> --delete #这里的remote_branch不用加origin
git rebase <branch> 把本分支的修改提交到目标分支上去，当两个分支都基于同一个祖先(base)有生长的时候，这种方法就可以修改(re)当前分支变动的祖先为目标分支了，前提是没有什么冲突；<branch>有很多种表达方式，可以是分支名，可以是HEAD，可以是commit_id，甚至可以是从某commit开始第几个(1s)（<branch>~<num>）
  -i # 可以编辑选择如何处理每一次commit，例如在出现的编辑器中pick保留第一个，后面的都改成s合并掉，就实现了压缩合并commit；如果rebase失败，则用git rebase --abort恢复
git reflog # 查看版本号变更记录
git remote add <name> <url> # 本地可以绑定好几个远程name，其中name如果是upstream，是用于当这个origin仓库是从这个upstream的仓库fork来的场景，这样，使用git fetch -p <name>的时候也能把多个远程代码都拉到本地；git remote remove <name>移除
git remote set-url origin # 修改git地址
git reset --hard 5693367 # 恢复成版本号以5693367开头的commit,(untracked file won't be change)
git stash save 'buf' #不提交，只暂存在本地
git stash list #查看有哪些暂存的
git stash pop stash@{<num>} #选择把哪个暂存的还原（前提是你要先切到当初stash save的分支上）
git submodule summary # 查看但是用的子模块的commit...此刻子模块的commit
git tag <name>;git push origin <name> #删除本地&远程 git tag -d <name>;git push origin :refs/tags/<name> #这里:前面是空的，就相当于推送一个空的tag
```
- linux ->WSL, Don't xvf in WSL:/mnt(destory the filemode), you can xvf and edit in WSL:~/$
- 配置了ssh的时候无需输入密码
- Personal access tokens生成项目令牌后可以写在脚本里git clone xxxxxxx<tokens>@<git-path> <branch>的方式无需输入密码
- 还有配置了credential.helper store的时候只需要输入一次密码（密码会保存到~/.git-credentials里，后续也无需输入密码了
- [清理曾经提交过的大垃圾文件方法](https://www.cnblogs.com/qinghe123/p/13230392.html?utm_source=tuicool)