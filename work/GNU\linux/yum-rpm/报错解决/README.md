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

#一般来说把Error: Package提到的卸掉即可