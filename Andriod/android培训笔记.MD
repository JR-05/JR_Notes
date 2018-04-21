JDK（Java Development Kit）：java开发工具包

SDK（Software Development Kit）：软件开发工具包。包含开发函数库，开发工具集

adt（Android develop tools）:安卓开发插件。配合eclipse使用

API(Application Programming Interface)):





##目录结构

### SDK目录结构

- add-one:额外功能附属文件（google地图）
- build-tools：编译工具
- docs:开发文档
- extras:驱动（usb，摄像头）
- platforms:Android 系统的jar版本(提供API开发函数库)
- platform-tools：平台开发工具（adb）
- sample ：示例代码
- sources：安卓系统源码
- system-imgaes ：系统镜像
- tools：开发用到的工具（模拟器）

模拟器相当于自己组装了一台新电脑，没有系统就是一个壳而已。那么它跟系统镜像合起来就组合成完整的手机。

###工程目录结构

- src:逻辑源代码
- gen:编译自动生成的代码
- Andriod 4.1.1：开发依赖SDKjar包
- assets:应用程序资源文件（MP3，Mp4）
- libs:第三方依赖库
- res

​       1.drawable:图片

​       2.layout:布局

​       3.menu:菜单

​       4.values:字符串

**values-v11和values-v14文件夹是干啥的？**

values-v11表示3.0及以上版本都会使用这里面的配置文件
同理，values-v14表示4.0及以上版本都会使用这里面的配置文件

- AndriodManifest.xml:应用程序的配置（包名，图标，名称）
- project.properties:应用程序编译版本配置文件



## Dalvik VM虚拟机

