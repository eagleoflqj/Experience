# 安装
* 官网下载解压
* 添加环境变量`flutter/bin`
* Android Studio安装flutter插件
```sh
flutter doctor # 检查
flutter doctor --android-licenses # 修复许可证问题
```
## sdkmanager --update报错
* 下载jaxb-api.jar、jaxb-runtime.jar、istack-commons-runtime.jar、activation.jar
* 打开sdkmanager，添加到`set CLASSPATH`尾部
# 设备
* Windows安装Google USB Driver
* flutter默认使用adb所属的Android SDK版本，设置`ANDROID_HOME`以覆盖
```sh
flutter devices
```
