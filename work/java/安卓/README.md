安装android studio（自带java环境、android sdk（AppData\Local\Android\Sdk））


- apktool
（要自己下载）
apk虽然能直接被unzip解压，但是apktool更强大，能反编译、解压、重打包；
apktool d解包后的目录结构是精心设计的。当你修改完代码和资源后，可以使用 apktool b  命令重新打包成一个新的 APK 文件；
java.exe -jar apktool_2.12.0.jar d game.apk -o decode\

- classes.dex
编译后的 Android 字节码文件。你的 Java/Kotlin 源代码最终都被编译成了这个格式
其他工具（如 dex2jar + jd-gui） JADX 可以直接将 classes.dex 反编译成可读的 Java 代码（尽管可能不是原始的完美代码）

- adb
（在sdk中，android sdk\platform-tools；有些其他工具也自带adb，例如scrcpy）

