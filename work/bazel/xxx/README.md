- new_git_repository好像跟git_repository没啥区别，有的说前者是依赖一个非bazel的仓库时，手动指定BUILD，但是后者好像也有这个能力？暂时只用git_repository；
- local_repository 
正常是，
```
git_repository(
   name = "xxx",
   remote = "https://x.com/y.git",
   init_submodules = True,
)
```
但是开发过程中可能频繁修改依赖的y.git；
所以可以先拉取到本地，然后临时换成下面的，类似golang中的replace作用，暂不支持init_submodules参数，所以要手动拉
```
local_repository(
    name = "xxx",
    path = "path/to/y", # 绝对路径和相对路径均可
)
```