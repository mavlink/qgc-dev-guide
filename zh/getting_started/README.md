# 初始步骤

本主题说明如何获取QGroundControl源代码并在本机或在Vagrant(虚拟机)环境中构建它。 它还提供有关可选或OS特定功能的信息。

## 源代码 

QGroundControl的源代码保存在GitHub上：https：//github.com/mavlink/qgroundcontrol。 它在Apache 2.0和GPLv3下是双重许可的。

如何获取源文件：

1. 克隆存储库 (或您的分叉), 包括子模块: ```git clone https://github.com/mavlink/qgroundcontrol.git --recursive```
2. 2.更新子模块（每次拉新源代码时都这样做）： ```git submodule update```

> 提示：不能使用Github以zip形式下载源文件，因为zip压缩包中不包含相应的子模块源代码。 你必须使用git工具！

## 构建QGroundControl开发环境

### 原生构建

OSG，Linux，Windows，iOS和Android支持QGroundControl构建。 QGroundControl使用Qt作为其跨平台支持库，并使用QtCreator作为其默认构建环境。

* macOS：v10.11或更高版本
* Ubuntu：64位，gcc编译器
* **Windows:** Vista or higher, [Visual Studio 2015 compiler](#vs2015) (32 bit)
* iOS：10.0及更高版本
* Android：Jelly Bean（4.1）及更高版本。 标准QGC是针对ndk版本19构建的。
* Qt版本：{{book.qt_version}}（仅限） <!-- NOTE {{ book.qt_version }} is set in the variables section of gitbook file https://github.com/mavlink/qgc-dev-guide/blob/master/book.json -->

> 提示: 有关更多信息，请参阅：Qt 5支持的平台列表。

#### Install Visual Studio 2015 (Windows Only) {#vs2015}

The Windows compiler can be found here: [Visual Studio 2015 compiler](https://visualstudio.microsoft.com/vs/older-downloads/) (32 bit)

When installing, you must minimally select all Visual C++ components as shown: ![Visual Studio 2015 - Select all Visual C++ Components](../../assets/getting_started/vs_2015_select_features.png)

#### Install Qt

You **need to install Qt as described below** instead of using pre-built packages from say, a Linux distribution, because *QGroundControl* needs access to private Qt headers.

To install Qt:

1. 下载并运行Qt Online Installer 
    * **Ubuntu:** 
        * 使用以下命令将下载的文件设置为可执行文件：chmod + x。 
        * Install to default location for use with **./qgroundcontrol-start.sh.** If you install Qt to a non-default location you will need to modify **qgroundcontrol-start.sh** in order to run downloaded builds.

2. In the installer *Select Components* dialog choose: {{ book.qt_version }}.
    
    Then install just the following components:

* **Windows**: *MCVC 2015 32 bit*
* **MacOS**: *macOS*
* **Linux**: *Desktop gcc 64-bit*
* All: 
    * *Qt Charts* and *Qt Remote Objects (TP)*
    * *Android ARMv7* (to build Android) 
        1. Install Additional Packages (Platform Specific)
* **Ubuntu:** `sudo apt-get install speech-dispatcher libudev-dev libsdl2-dev`
* **Fedora:** `sudo dnf install speech-dispatcher SDL2-devel SDL2 systemd-devel`
* **Arch Linux:** `pacman -Sy speech-dispatcher`
* **Windows:** [USB Driver](http://www.pixhawk.org/firmware/downloads) to connect to Pixhawk/PX4Flow/3DR Radio
* **Android:** [Qt Android Setup](http://doc.qt.io/qt-5/androidgs.html)

#### Building using Qt Creator

1. 启动Qt Creator并打开qgroundcontrol.pro项目。
2. 2. 根据您的需求选择合适的套件： 
    * OSX：桌面Qt {{book.qt_version}} clang 64 bit>注意iOS构建必须使用XCode构建。
    * Ubuntu：桌面Qt {{book.qt_version}} GCC位
    * **Windows:** Desktop Qt {{ book.qt_version }} MSVC2015 **32bit**
    * Android：Android：适用于armeabi的Android-v7a（GCC 4.9，Qt {{book.qt_version}}）

3. Build using the "hammer" (or "play") icons:
    
    ![QtCreator Build Button](../../assets/getting_started/qt_creator_build_qgc.png)

### Vagrant

[Vagrant](https://www.vagrantup.com/) can be used to build and run *QGroundControl* within a Linux virtual machine (the build can also be run on the host machine if it is compatible).

1. 1. 下载并安装Vagrant
2. 2. 从QGroundControl存储库的根目录运行vagrant up
3. 3 .为了使用图形环境，请运行vagrant reload

### 所有支持操作系统的附加构建说明

* 警告为错误：指定CONFIG = WarningsAsErrors会将所有警告转换为错误，从而破坏构建。 如果您正在处理拉取请求，您计划提交给github进行考虑，则应始终在启用此设置的情况下运行，因为所有拉取请求都需要此设置。 >注意将此行放入顶级目录（与qgroundcontrol.pro相同的目录）中名为user_config.pri的文件中，将在所有构建上设置此标志，而不会干扰GIT历史记录。
* 并行构建：对于非Windows构建，您可以使用-j＃选项来运行并行构建。
* 构建文件的位置：可以在build_debug或build_release目录中找到单个构建文件结果。 可以在debug或release目录中找到构建的可执行文件。
* 如果在运行QGroundControl时遇到此错误：/usr/lib/x86_64-linux-gnu/libstdc++.so.6：找不到版本'GLIBCXX_3.4.20'，则需要更新到最新的gcc，或安装最新的 libstdc ++。6使用：sudo apt-get install libstdc ++ 6。
* 单元测试：要运行单元测试，使用UNITTEST_BUILD定义构建调试模式，然后在运行测试之前将deploy / qgroundcontrol-start.sh脚本复制到调试目录中。

## 可选/特定于操作系统的功能

*QGroundControl* has functionality that is dependent on the operating system and libraries installed by the user. The following sections describe these features, their dependencies, and how to disable/alter them during the build process. These features can be forcibly enabled/disabled by specifying additional values to qmake.

### Opal-RT的RT-LAB模拟器

Integration with Opal-RT's RT-LAB simulator can be enabled on Windows by installing RT-LAB 7.2.4. This allows vehicles to be simulated in RT-LAB and communicate directly with QGC on the same computer as if the UAS was actually deployed. This support is enabled by default once the requisite RT-LAB software is installed. Disabling this can be done by adding `DEFINES+=DISABLE_RTLAB` to qmake.

### XBee支持

*QGroundControl* can talk to XBee wireless devices using their proprietary protocol directly on Windows and Linux platforms. This support is not necessary if you're not using XBee devices or aren't using their proprietary protocol. On Windows, the necessary dependencies are included in this repository and no additional steps are required. For Linux, change to the `libs/thirdParty/libxbee` folder and run `make;sudo make install` to install libxbee on your system (uninstalling can be done with a `sudo make uninstall`). *qmake* will automatically detect the library on Linux, so no other work is necessary.

To disable XBee support you may add `DEFINES+=DISABLE_XBEE` to *qmake*.

### 视频流 

Check the [Video Streaming](https://github.com/mavlink/qgroundcontrol/tree/master/src/VideoStreaming) directory for further instructions.

## 下载最新的开发版本

QGroundControl mantains this download links to allow test and usage of the last updates in main code.

* [Windows (QGroundControl-installer.exe)](https://s3-us-west-2.amazonaws.com/qgroundcontrol/builds/master/QGroundControl-installer.exe)
* [MAC OS (QGroundControl.dmg)](https://s3-us-west-2.amazonaws.com/qgroundcontrol/builds/master/QGroundControl.dmg)
* [Linux（QGroundControl.AppImage）](https://s3-us-west-2.amazonaws.com/qgroundcontrol/builds/master/QGroundControl.AppImage)
* [Android (QGroundControl.apk)](https://s3-us-west-2.amazonaws.com/qgroundcontrol/builds/master/QGroundControl.apk)