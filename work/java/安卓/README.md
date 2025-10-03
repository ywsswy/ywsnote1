安装android studio（自带java环境、android sdk（AppData\Local\Android\Sdk））


- apktool
（要自己下载）
apk虽然能直接被unzip解压，但是apktool更强大，能反编译、解压、重打包；
apktool d解包后的目录结构是精心设计的
java.exe -jar apktool_2.12.0.jar d game.apk -o decode/
当你修改完代码和资源后（META-INF文件的不需要修改，apktool会自动帮改），可以使用 apktool b  命令重新打包成一个新的 APK 文件；
java.exe -jar apktool_2.12.0.jar b decode/ -o game2.apk
此时还必须签名
<apk build-tool> zipalign.exe -p -f 4 game2.apk game3.apk
<in studio> ..\???\<to_apk build_tool>  apksigner.bat sign --ks my-release-key.jks --ks-key-alias my-key-alias --ks-pass pass:123456 --key-pass pass:123456 --out game4.apk game3.apk

- jks生成方法：
<studio> keytool.exe -genkey -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias my-key-alias

- classes.dex
编译后的 Android 字节码文件。你的 Java/Kotlin 源代码最终都被编译成了这个格式
其他工具（如 dex2jar + jd-gui） JADX 可以直接将 classes.dex 反编译成可读的 Java 代码（尽管可能不是原始的完美代码）

- adb
（在sdk中，android sdk\platform-tools；有些其他工具也自带adb，例如scrcpy）

- jadx-gui
（要github下载）
可以反汇编classes.dex文件变成java（仅用于分析代码，但会丢失信息）

- 想调试apk的话，
先apktool 解包，然后修改AndroidManifest.xml，在<application 这里> 添加android:debuggable="true"

- 想root安卓模拟器的话
android studio 启动一个google api的就行，不要启动google play的

- ida
逆向代码
下载：https://www.52pojie.cn/forum.php?mod=viewthread&tid=1581672&extra=page%3D1&page=1

- 想ida pro调试安卓模拟器的话（没跑通）
需要root安卓模拟器，并且把android_server64用adb传过去，然后adb shell 过去./启动
然后用adb启动（等待debug模式的）app
adb shell am start -D -ncom.tianxaing.release/com.fish.main.MainGameActivity





## 一些参考
- https://blog.csdn.net/qq_42177292/article/details/121261627?spm=1001.2101.3001.6650.10&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-10-121261627-blog-147095615.235%5Ev43%5Epc_blog_bottom_relevance_base4&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-10-121261627-blog-147095615.235%5Ev43%5Epc_blog_bottom_relevance_base4&utm_relevant_index=19

- 修改smail
https://blog.csdn.net/weixin_37618354/article/details/149749516

- https://www.freebuf.com/author/zzwlpx
