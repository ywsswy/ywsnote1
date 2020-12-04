adb devices

adb push d:\test.txt /sdcard/

adb logcat | find "LocationManager"

adb shell

adb shell pm list package

adb shell dumpsys window | findstr mCurrentFocus

adb install <apk 绝对路径>