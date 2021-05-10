首先A项目有一个git仓库
然后B项目需要依赖于这A
则在B中，指定某个文件夹下放A模块
git submodule add <git address of A> <path in B>




别人使用的时候
先clone B，然后在B中
git submodule update --init --recursive


就算A变了，但是如果B不变，别人clone了B，git submodule update也是B的效果，而不是A最新的效果？？？？？？？？？？？？未必吧

git submodule update --init --recursive
git submodule foreach --recursive git checkout master
git submodule foreach git pull



## 场景1：新拉的项目必须
git submodule init #然后status 显示的是子模块所需要的commit，前面有减号
git submodule update # status 就没有减号了

## 场景2：子模块自己的commit基础上，做了修改但并未提交
无需任何操作，修改不会丢失

## 场景3：已经把子模块切到其他commit了/或者修改已经提交了
会检出到正确的commit上，提交的也不会丢失

## 场景4：在其他分支上修改未提交
git submodule update不会成功

综上，这两个即可，啥都不怕
git submodule init
git submodule update
可以用一条命令替换
git submodule update --init --recursive



# 删除

git submodule deinit <sub>
git rm <sub> #这个就能移除.gitmodule里面的内容了

???删除submodule可能删不干净，方法是要删掉后git rm --cached xxx