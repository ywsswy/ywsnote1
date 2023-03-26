[图解](https://images2015.cnblogs.com/blog/809218/201606/809218-20160604213832164-1203726937.png)
[git代码管理工具学习](https://git-scm.com/book/zh/v2)
commit message规范 ：feat(merge) fix | build style revert docs refactor | test perf
git diff <old commited/branch> <new commited/new brand> [<file_name>]#注意此刻的untracked file不会参与到diff中
git diff # 常规diff（当前与暂存区比较）
git diff HEAD # 如果有暂存区等影响如merge stash_pop等，则用这个，或者发现git diff异常的时候，stash_save_then_pop就发现又好了
  参数--filter=M 仅显示更改的文件，不知道新增的文件
  参数--name-status 不管详细内容，只管文件是A/D/M
git branch -D <local_branch> #删除本地分支
git branch -m '重命名分支名'
git push origin <remote_branch> --delete #这里的remote_branch不用加origin
git checkout <branch_name> #切换分支。一是使 HEAD 指回 branch_name分支，二是将工作目录恢复成 branch_name分支所指向的快照内容
git checkout <other branch_name> -- <file>#把那个分支的文件覆盖过来 666
git log # 查看历次详细commit eg:commit 56933675c782a232668c6fcbfffd249625c93947(版本号)
git log --graph --decorate --oneline --all #这个是最详细(--simplify-by-decoration只看分支粒度的改动)
git config --global user.name "ywsswy"
git config --global user.email "ql1119079560@yahoo.com"
git reflog # 查看版本号变更记录
git reset --hard 5693367 # 恢复成版本号以5693367开头的commit,(untracked file won't be change)
git ls-files #查看仓库了有哪些文件
git stash save 'buf' #不提交，只暂存在本地
git stash list #查看有哪些暂存的
git stash pop stash@{<num>} #选择把哪个暂存的还原（前提是你要先切到当初stash save的分支上）
git fetch -p 获取所有远程分支改动信息（p可以保证删掉的远程分支不再显示）（如果某个分支显示 xxxxxx...yyyyyyy br1  (forced update) 表明有人force到yyyyyyy，xxxxx没有参考意义！）
git tag <name> # git tag -d <name> # git push origin <name>
git commit --amend #修改commit信息
git submodule summary # 查看但是用的子模块的commit...此刻子模块的commit

- linux ->WSL, Don't xvf in WSL:/mnt(destory the filemode), you can xvf and edit in WSL:~/$
- git rebase -i HEAD~<num> # 把最近num条commit合并成一个commit，
- 出现的一个编辑器，pick为保留的commit，s为被合并掉的commit，写在下面的几个就改成s
- 如果压缩失败，则用git rebase --abort恢复
- git merge出现冲突的时候使用git mergetool # diffg L/R可以选择采用本地/远程的修改，]/[c可以跳到下/上一处修改
- 我建议mergetool完之后进行一次全搜<======，<<<<<<<，=======，>>>>>>>

- Personal access tokens生成项目令牌后可以写在脚本里git clone xxxxxxx<tokens>@<git-path> <branch>的方式无需输入密码