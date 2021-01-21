只有项目里面还有文件夹才算有子包，跟目录的不算子包

所以import的时候只能写到根目录

同时根目录的包名如果跟根目录名不相同，就必须对根目录做别名

import <package> <path>
...
var v <package>...