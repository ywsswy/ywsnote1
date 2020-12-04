# usage

Project-> New Project（仅仅需要改名字）-> Setting（check “Store function-local symbols in database.”（个人觉得如果Project Source Directory可以设置好，以便头文件能找到，还可以放一些应该Include的进来） -> Add and Remove Project Files（如果不在C盘，就在第一个框输入D:，然后找到自己要加入进来的项目文件夹单击，然后Add Tree，然后close）-> Project.Rebuild Project
Project-> New Project（仅仅需要改名字）->Project Source Directory设置好源文件的主目录，一定要Add Tree,然后其他想Include的目录（Project -> import external symbols -> add(import from a source code tree)），

* 显示代码行号
Options ->Document Options->（这里面每个文档类型都要设置，C++类型的别忘了加一个.cc文件）-> Check Show line numbers
Options ->File Type Options

* 豆沙绿背景色（保护眼睛/doge）
Preferences -> colors -> Background -> Color:(204,232,207)

* 不要项目路径窗口首字母大写
Preferences ->Display -> Show exact case of file names。

* 3.* 版本不支持utf-8，所以创建文件先用Notepad++创建 tab键会乱码，所以document option设置不要显示tab键
* 4.0 版本可以（但是4.0。。。。不适合我们的项目）


# 如果项目文件夹下放了新文件，可以Project > Synchronize Files来更新

* 全选快捷键ctrl+A
https://zhidao.baidu.com/question/402491241.html

* 关于代码规范，缩进 options->document options-> check（Expand tabs）& click（Auto Indent）->none （可能会无效，把Default的格式也改一下）