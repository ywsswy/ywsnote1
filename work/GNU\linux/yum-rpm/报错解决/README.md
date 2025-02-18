## 错误一
--> Processing Dependency: <库> for package: <完整包名>

name-VERSION-release.arch.rpm
---> Package <name.arch> <VERSION-release now> will be updated
---> Package <name.arch> <VERSION-release need> will be an update
---> Package <name.arch> <VERSION-release need> will be installed #从来没装过的

--> Finished Dependency Resolution

Error: Package: <完整包名 need>
           Requires: <该need包的里面spec写的依赖条件>
           Available/Updated By/Removing: <说明，不满足该条件，要进行的变动>
               <相关包>

// 一般来说把Error: Package提到的卸掉即可

## 错误二
--> Processing Dependency: <yy-v1> for package: <xx-v1>
--> Processing Dependency: <yy-v2> for package: <xx-v2>

---> Package <yy-v1> will be installed
---> Package <yy-v2> will be installed

Error:  Multilib version problems found...
       Protected multilib versions: <yy-v1> != <yy-v2>

// xx是已经安装了的，有两个版本，本次安装会把依赖的yy也安装上，但是两个版本的yy会冲突，所以需要先把xx不对的那个版本卸载掉

## 错误三

Transaction check error:
  file <yy> from install of <yy-v1> conflicts with file from package <yy-v2>

// 本机已经有了yy文件（是yy-v2包安装的），但是本次安装<yy-v1>时也想安装yy文件，所以冲突了，所以需要把yy-v2卸载掉