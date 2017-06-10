---
title: 在 Windows7 平台搭建 Cocos2d-x 开发环境
date: 2017-06-08 23:41:25
tags:
- Cocos2d-x
categories: 
- Cocos2d-x
---

在这里向 Coccos2d-x 新手简单介绍一下如何在 Windows 7 平台上搭建 Cocos2d-x 开发环境，老手、大牛和大神请跳过。其他平台的开发环境搭建也可以参考本文，只需要安装对应平台软件和配置环境变量即可。本文使用 Cocos2d-x 3.15 版本，但对于 Cocos2d-x 3.0 以上版本的开发环境搭建应该也是适用的，可能 NDK 的版本需要稍作改动，请自行尝试。<!--more-->

# 系统环境

Windows 7 64位；Cocos2d-x 3.15

# 下载 Cocos2d-x 3.15

下载 [Cocos2d-x 3.15](http://www.cocos.com/download)  并解压。

查看 Cocos2d-x 3.15 目录下的 README.md 文件，你会发现有以下说明：

````
Build Requirements
------------------

* Mac OS X 10.7+, Xcode 7+
* or Ubuntu 12.10+, CMake 2.6+
* or Windows 7+, VS 2013+
* Python 2.7.5
* NDK r11+ and API level 19 is required to build Android games
* Windows Phone/Store 8.1 VS 2013 Update 4+ or VS 2015
* Windows Phone/Store 10.0 VS 2015
* JRE or JDK 1.6+ is required for web publishing
````

# 安装 Visual Studio 2013+

下载安装 Visual Studio 2013 以上版本，因为Cocos2d-x-v3.x引擎不能用老版本的 VS 编译（貌似可以使用 VS 2012，请自行验证）。安装的时候如果你只使用 VS 当做 Cocos2d-x 开发工具，那么你可以只安装 c++ 开发相关的功能，这样可以节省空间和减少安装时间。

# 安装 Python

下载 [Python 2.7.3](http://www.python.org/download/releases/2.7.3/) 并安装（虽然 README.md 中写着 Python 2.7.5 ，亲测可用 Python 2.7.3版本），安装后把 `D:\tools\Python27;` 添加到系统环境变量 Path 中，最后打开  cmd 控制台输入 “python -V” 检查是否安装成功并配好环境变量。

注意：由于 Python 2.x 和 Python 3.x 是不兼容的，所以如果安装了 Python 3.x 是应该是有问题的，请自行验证。

# 安装 Java JDK

根据你的系统平台下载对应的 [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 并安装，我这里下载的是 Windows 7  64位对应的安装包。

1. 新建环境变量：JAVA_HOME 值为：`D:\Program Files\Java\jdk1.7.0`
2. 新建环境变量：CLASSPATH  值为：`.;%JAVA_HOME%\lib;` （注意：点号表示当前目录，不能省略）
3. 在系统环境变量 Path 中加入：`%JAVA_HOME%\bin;` （注意:这里的分号不能省略）
4. 打开 cmd 控制台输入 “java -version" 检查是否安装并配置好环境变量。

注意：JDK 最好使用 1.6 以上版本，使用 1.5 版本可能会有问题，请自行验证。

# 安装 Android SDK

下载 [ADT Bundle 23.0.0](http://www.androiddevtools.cn/) ，解压并把文件夹名称修改为 ”adt-bundle“。ADT Bundle包含了Eclipse、ADT 插件和 SDK Tools，推荐初学者下载ADT Bundle，不用再折腾开发环境。

1. 新建环境变量：ANDROID_SDK 值为：` D:\tools\adt-bundle\sdk\platforms;D:\tools\adt-bundle\sdk\platform-tools;D:\tools\adt-bundle\sdk\tools;`
2. 打开 cmd 控制台输入 “adb -h" 检查是否安装并配置好环境变量。

注意：我下载的是 ADT Bundle，里面的 SDK 才是必须要用到的，但在开发过程中总会有需要用到 Eclipse 的时候。你也可以单独下载 SDK，然后在需要修改 Android 代码或者调试 Android 的时候使用其他 IDE，例如 Android Studio。

# 安装 Android NDK

下载 [android-ndk-r11c](https://developer.android.google.cn/ndk/downloads/older_releases.html#ndk-10c-downloads) 并解压。

新建环境变量：NDK_ROOT 值为：`D:\tools\android-ndk-r11c`

# 安装 ant

下载 [apache-ant](http://ant.apache.org/) 并解压。

新建环境变量：ANT_ROOT 值为：`D:\tools\apache-ant-1.9.7\bin`

# 配置 cocos2d-console

cocos2d-console 是 Cocos2d-x 的命令行工具，使用它可以很方便地创建、编译和发布 Cocos2d-x 工程。

打开 cmd 控制台，输入 `cd /d D:\tools\cocos2d-x-3.15`  进入引擎根目录，然后输入 `python setup.py` 执行 Cocos2d-x 配置文件 setup.py ，根据提示输入对应的 SDK、NDK、ant 等路径完成 Cocos2d-x 环境变量设置（应该只用输入部分路径，根据提示输入即可）。完成配置后就可以使用 cocos2d-console 了，详细的使用说明请查看[官方 cocos2d-console 使用说明文档](http://www.cocos2d-x.org/wiki/Cocos2d-console)。

最后打开 cmd 控制台输入 “cocos -v” 检查 Cocos2d-x 环境变量是否已经设置正确。

# 新建 Cocos2d-x 工程

打开 cmd 控制台，输入 `cd /d D:\myproj` 进入存放新工程的目录，然后输入 `cocos new TestLua -l lua -p org.cocos2dx.mygame` 创建一个新的 Cocos2d-x lua 工程。

使用 VS 打开 `D:\myproj\TestLua\frameworks\runtime-src\proj.win32` 下的 VS 工程，然后就可以在 VS 中运行和调试 Cocos2d-x 工程了。

PS：至于如果运行官方示例 Cocos2d-x test-cpp，请参考[官方文档](http://www.cocos2d-x.org/wiki/How_to_run_cpp-tests_on_Windows)。

# 打包 Android apk

* 使用 cocos2d-console 打包：

打开 cmd 控制台并进入到刚刚新建的 Cocos2d-x 工程根目录，然后输入 `cocos compile -p android -m debug ` 就可以编译出 debug 版本的 Android apk 了。更详细的打包说明请查看[官方 cocos2d-console 使用说明文档](http://www.cocos2d-x.org/wiki/Cocos2d-console)。

* 使用 Eclipse 打包：

按照下图导入 Cocos2d-x Android 相关工程：`D:\myproj\TestLua\frameworks\cocos2d-x\cocos\platform\android\java` 以及 `D:\myproj\TestLua\frameworks\runtime-src\proj.android`。然后使用 Eclipse 打包 apk 即可。

![](http://o9sn2y8lr.bkt.clouddn.com/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20170606202257.png)

![](http://o9sn2y8lr.bkt.clouddn.com/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20170606202439.png)

![](http://o9sn2y8lr.bkt.clouddn.com/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20170606202514.png)



打包 apk 过程中可能会遇到找不到对应的 Android API 版本问题，这是由于 Android SDK 中未按照对应的 Andriod 引起的。解决方法有以下两种：

1. 查看 Android SDK 中的 platforms 目录存在的 Android 版本是多少，例如 platforms 目录下存在 `android-22`。那么就把 `D:\myproj\TestLua\frameworks\runtime-src\proj.android\project.properties ` 和 `D:\myproj\TestLua\frameworks\cocos2d-x\cocos\platform\android\java\project.properties ` 文件中的 `target=android-22` 都设置成 SDK 中已下载好的 Android 版本。
2. 运行 `D:\tools\adt-bundle\SDK Manager.exe`，并下载对应的 Android 版本。下载过程可能需要翻墙，这时就需要按以下方法设置代理：
> Android SDK在线更新镜像服务器
>
> 1. 中国科学院开源协会镜像站地址:
>
>    - IPV4/IPV6: `mirrors.opencas.cn` 端口：80
>    - IPV4/IPV6: `mirrors.opencas.org` 端口：80
>    - IPV4/IPV6: `mirrors.opencas.ac.cn` 端口：80
>
> 2. 上海GDG镜像服务器地址:
>
>    `sdk.gdgshanghai.com` 端口：8000
>
> 3. 北京化工大学镜像服务器地址: 
>
>    - IPv4: `ubuntu.buct.edu.cn/` 端口：80
>    - IPv4: `ubuntu.buct.cn/` 端口：80
>    - IPv6: `ubuntu.buct6.edu.cn/` 端口：80
>
> 4. 大连东软信息学院镜像服务器地址: 
>
>    `mirrors.neusoft.edu.cn` 端口：80
>
> 5. 郑州大学开源镜像站: 
>
>    `mirrors.zzu.edu.cn` 端口：80
>
> **使用方法：**
>
> 1. 启动 Android SDK Manager ，打开主界面，依次选择『**Tools**』、『**Options...**』，弹出『**Android SDK Manager - Settings**』窗口；
>
> 2. 在『**Android SDK Manager - Settings**』窗口中，在『**HTTP Proxy Server』和『HTTP Proxy Port**』输入框内填入上面镜像服务器地址(**不包含http://**，如下图)和端口，并且选中『**Force https://... sources to be fetched using http://...**』复选框。设置完成后单击『**Close**』按钮关闭『**Android SDK Manager - Settings**』窗口返回到主界面；
>
> 3. 依次选择『**Packages**』、『**Reload**』。
>
>    ![SDK Manager Proxy Settings](http://www.androiddevtools.cn/static/image/sdk-manager-proxy-settings.png)
>
>

以上文字从 http://www.androiddevtools.cn/ 引用。

# 可能会遇到的错误

* VS 运行 Cocos2d-x 工程出错：

```
Can't create window
More info:
GLFWError #65537 Happen,The GLFW library is not initialized
```

解决方法：更新电脑显卡驱动，一般是新安装的系统会出现这种错误。

* 打包 Cocos2d-x lua 工程出错：

``` 
luajit-win32.exe
无法启动此程序，因为计算机中丢失 VCRUNTIME140.dll。尝试重新安装该程序以解决此问
```

解决方法：下载  [vc++2015 ](http://www.cnblogs.com/xxoome/p/5612411.html) 并安装即可。

> 本文出自 [Eddy Wiki](http://eddy.wiki) ，转载请注明出处：[http://eddy.wiki/setup-cocos2d-x.html](http://eddy.wiki/setup-cocos2d-x.html)