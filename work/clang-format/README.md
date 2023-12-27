这个跟vscode没有必然联系

vscode可以安装这个插件，settings.json 中可以设置一部分自定义值

但是真正的配置文件要写到.clang-format里面，这个建议在home目录写，执行文件format时是逐层往上直到找到该配置文件为止


// clang-format off
在这中间的代码不会被格式化
// clang-format on