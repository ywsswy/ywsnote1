extensions
enable developer mode
查看某插件id(eg. dbepggeogbaibhgnhhndojpepiihcmeb)
pack extension
root directory(C:/user/<user_name>/appdata/local/google/chrome/user data/default/extensions/<id>/<version_id>）
（这个appdata是隐藏目录）
the .crx and .pem file will created at the <id> file

crx文件直接拖拽到新电脑即可安装（或者把crx文件重命名为rar文件解压后load该目录）


# 插件开发
https://www.cnblogs.com/liuxianan/p/chrome-plugin-develop.html

开发者模式
首先把文件夹弄出来，metadata里好像有hash校验，把metadata文件夹删掉，加载文件夹，每次修改后，在Extensions页面点击reload按钮即可生效
从background.js改起

contentjs是直接往页面注入的js

消息通信post


## 安装viminum
https://github.com/philc/vimium/blob/master/CONTRIBUTING.md#installing-from-source

## 安装 SwitchyOmega_Chromium
https://blog.csdn.net/zhangfenger/article/details/126432142