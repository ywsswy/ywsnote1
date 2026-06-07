- js引擎：能识别js代码；

- node.js 是一个 js 运行时环境，基于 Chrome V8 引擎（一种js引擎）构建；能让js代码脱离浏览器，是在服务器运行的，像vscode-ssh其实就是在远程常驻运行了个node.js进程；

- js运行时环境：js引擎 + 一些配套工具（配套工具例如有console.log的注入实现，不同运行时环境consoloe.log的配套实现不一致，例如浏览器里是输出到控制台，node.js里是输出到终端）；

- npm（Node Package Manager）：用于管理/安装js依赖包，node.js内置npm；

```
npm init  # 会在项目里创建一个package.json文件
npm config set registry http://registry.npm.taobao.org/  # 将 npm 的包下载源（registry）切换为淘宝镜像（默认的 https://registry.npmjs.org/，国内不够快），这个命令会更新用户级别的 npm 配置文件（~/.npmrc 文件）
```

- npx：是一个npm包执行工具，用于“临时”下载包，执行用完即卸载，不污染全局环境（可以直接执行 npm 包，而无需全局安装），（npm 5.2+ 开始内置npx）

- nvm（Node Version Manager）：是一个 node.js 版本管理工具；（不同项目可能依赖不同版本的 node.js，nvm 可让同一台机器上安装和切换多个版本（通过.nvmrc 文件））

```
nvm install 18        # 安装 node.js 18
nvm use 18            # 切换到 node.js 18
nvm ls                # 查看已安装的版本
nvm alias default 18  # 设置默认版本 
```

- volta：是一个 node.js 版本管理工具，类似nvm，在package.json文件里指定；

```
volta pin node@24.14.1  # 设置node.js默认版本，会写到package.json文件里；

安装自己：curl https://get.volta.sh | bash
安装node.js volta install node
```


