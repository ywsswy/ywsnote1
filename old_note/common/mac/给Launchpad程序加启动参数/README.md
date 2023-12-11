Automator中“创建新文稿”（Create New Document）。
选择“应用程序”（Application）类型，然后点击“选择”（Choose）。
在左侧的“库”（Library）列表中，找到“运行Shell脚本”（Run Shell Script）
在“运行Shell脚本”（Run Shell Script）框中，将“Shell”设置为/bin/bash，
并将“传递输入”（Pass input）设置为“作为参数”（as arguments）。

在文本框中，输入以下内容（表示启动chrome程序，并且添加了启动参数--xxx=666）：
open -a "Google Chrome" --args --xxx=666
保存到“应用程序”（Applications）文件夹，后面就可以在launchpad中找到这个了

如果想改应用图标，可以应用文件夹，右键应用程序，show package contents，把Contents/Resources中的icns文件替换掉