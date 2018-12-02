# 初始步骤

本主题说明如何获取QGroundControl源代码并在本机或在Vagrant(虚拟机)环境中构建它。 本主题还提供其他可选功能信息及特定于操作系统的功能信息。

## Daily Builds

If you just want to test (and not debug) a recent build of *QGroundControl* you can use the [Daily Build](https://docs.qgroundcontrol.com/en/releases/daily_builds.html). Versions are provided for all platforms.

## Source Code

Source code for *QGroundControl* is kept on GitHub here: https://github.com/mavlink/qgroundcontrol. It is [dual-licensed under Apache 2.0 and GPLv3](https://github.com/mavlink/qgroundcontrol/blob/master/COPYING.md).

To get the source files:

1. 克隆存储库 (或您的分叉), 包括子模块: ```git clone https://github.com/mavlink/qgroundcontrol.git --recursive```
2. 2.更新子模块（每次拉新源代码时都这样做）： ```git submodule update```

> 提示：不能使用Github以zip形式下载源文件，因为zip压缩包中不包含相应的子模块源代码。 你必须使用git工具！

## Build QGroundControl

### 原生构建

*QGroundControl* builds are supported for macOS, Linux, Windows, iOS and Android. *QGroundControl* uses [Qt](http://www.qt.io) as its cross-platform support library and uses [QtCreator](http://doc.qt.io/qtcreator/index.html) as its default build environment.

* macOS：v10.11或更高版本
* Ubuntu：64位，gcc编译器
* **Windows:**vista 或更高版本, < 1>Visual studio 2015 编译器 </1 > (32位)
* iOS：10.0及更高版本
* Android：Jelly Bean（4.1）及更高版本。 标准QGC是针对ndk版本19构建的。 标准QGC是针对ndk版本19构建的。
* ** Qt版本：</ 0> {{book.qt_version}} **（仅限）</ 0> <!-- NOTE {{ book.qt_version }} is set in the variables section of gitbook file https://github.com/mavlink/qgc-dev-guide/blob/master/book.json --></li> </ul> 
    
    > 提示: 有关更多信息，请参阅：Qt 5支持的平台列表。
    
    #### 安装 visual studio 2015 (仅限 windows) {#vs2015}
    
    The Windows compiler can be found here: [Visual Studio 2015 compiler](https://visualstudio.microsoft.com/vs/older-downloads/) (32 bit)
    
    When installing, you must minimally select all Visual C++ components as shown: ![Visual Studio 2015 - Select all Visual C++ Components](../../assets/getting_started/vs_2015_select_features.png)
    
    #### 安装Qt
    
    You **need to install Qt as described below** instead of using pre-built packages from say, a Linux distribution, because *QGroundControl* needs access to private Qt headers.
    
    To install Qt:
    
    1. 下载并运行[Qt Online Installer](http://www.qt.io/download-open-source) 
        * **Ubuntu:** 
            * 使用以下命令将下载的文件设置为可执行文件：`chmod + x` 
            * 请安装到默认位置, 以便与 **./qgroundcontrol-start.sh** 一起使用。如果将 Qt 安装到非默认位置, 则需要修改 **qgroundcontrol-start.sh** ，才能运行下载的组件。
    
    2. 在安装程序 的*Select 组件 </0 > 对话框中, 选择 {{ book.qt_version }}。</p> 
        
        然后，按如下向导，安装组件:</li> </ol> 
        
        * **Windows**: *MCVC 2015 32 bit*
        * **MacOS**: *macOS*
        * **Linux**: *Desktop gcc 64-bit*
        * 必装组件（所有平台） 
            * *Qt Charts* and *Qt Remote Objects (TP)*
            * *Android ARMv7* (为了构建Android) 
                1. 安装附加软件包（特定于平台）
        * **Ubuntu:** `sudo apt-get install speech-dispatcher libudev-dev libsdl2-dev`
        * **Fedora:** `sudo dnf install speech-dispatcher SDL2-devel SDL2 systemd-devel`
        * Arch Linux: pacman -Sy speech-dispatcher
        * Windows: USB Driver to connect to Pixhawk/PX4Flow/3DR Radio
        * **Android:** [Qt Android Setup](http://doc.qt.io/qt-5/androidgs.html)
        
        #### 使用Qt Creator构建
        
        1. 启动*Qt Creator*并打开**qgroundcontrol.pro**项目。
        2. 根据您的需求选择合适的套件： 
            * OSX：桌面Qt {{book.qt_version}} clang 64 bit>注意iOS构建必须使用XCode构建。
            * Ubuntu：桌面Qt {{book.qt_version}} GCC位
            * **Windows:** 桌面Qt{{ book.qt_version }}MSVC2015**32bit**
            * **Android：** Android平台需选择armeabi的Android-v7a（GCC 4.9，Qt {{ book.qt_version }}）
        
        3. 使用"hammer" (or "play") 图标构建:
            
            ![QtCreator构建按键](../../assets/getting_started/qt_creator_build_qgc.png)
        
        ### Vagrant
        
        [Vagrant](https://www.vagrantup.com/) can be used to build and run *QGroundControl* within a Linux virtual machine (the build can also be run on the host machine if it is compatible).
        
        1. 1. 下载并安装Vagrant
        2. 2. 从QGroundControl存储库的根目录运行vagrant up
        3. 3 .为了使用图形环境，请运行vagrant reload
        
        ### 所有支持操作系统的附加构建说明
        
        * **Warnings as Errors:** 指定`CONFIG = WarningsAsErrors`会将所有警告转换为错误，从而使得构建程序无法顺利执行。 如果您正在处理拉取请求，您计划提交给github进行考虑，则应始终在启用此设置的情况下运行，因为所有拉取请求都需要此设置。 **注意：**将此行放入顶级目录（与**qgroundcontrol.pro**相同的目录）中名为**user_config.pri**的文件中，将在所有构建上设置此标志，而不会干扰GIT历史记录。
        * **并行构建：** 对于非Windows系统下的构建，您可以使用`-j＃`选项来运行并行构建。
        * **构建文件的位置：** 可以在`build_debug`或`build_release`目录中找到单个构建文件结果。 可以在`debug`或`release`目录中找到构建的可执行文件。
        * **如果在运行*QGroundControl*时出现报错：** `/usr/lib/x86_64-linux-gnu/libstdc++.so.6: version 'GLIBCXX_3.4.20' not found.` ，则需要更新到最新的*gcc*，或安装最新的*libstdc++.6* ：`sudo apt-get  install  libstdc ++ 6 ` 。
        * **单元测试：** 如需运行[unit tests](../contribute/unit_tests.md),请使用`UNITTEST_BUILD`定义 `debug`模式，然后在运行测试之前将`deploy / qgroundcontrol-start.sh`脚本文件复制到 `debug`目录中。
        
        ## Optional/OS-Specific Functionality
        
        *QGroundControl* has functionality that is dependent on the operating system and libraries installed by the user. The following sections describe these features, their dependencies, and how to disable/alter them during the build process. These features can be forcibly enabled/disabled by specifying additional values to qmake.
        
        ### Opal-RT的RT-LAB模拟器
        
        Integration with Opal-RT's RT-LAB simulator can be enabled on Windows by installing RT-LAB 7.2.4. This allows vehicles to be simulated in RT-LAB and communicate directly with QGC on the same computer as if the UAS was actually deployed. This support is enabled by default once the requisite RT-LAB software is installed. Disabling this can be done by adding `DEFINES+=DISABLE_RTLAB` to qmake.
        
        ### XBee支持
        
        *QGroundControl* can talk to XBee wireless devices using their proprietary protocol directly on Windows and Linux platforms. This support is not necessary if you're not using XBee devices or aren't using their proprietary protocol. On Windows, the necessary dependencies are included in this repository and no additional steps are required. For Linux, change to the `libs/thirdParty/libxbee` folder and run `make;sudo make install` to install libxbee on your system (uninstalling can be done with a `sudo make uninstall`). *qmake* will automatically detect the library on Linux, so no other work is necessary.
        
        To disable XBee support you may add `DEFINES+=DISABLE_XBEE` to *qmake*.
        
        ### 视频流 
        
        Check the [Video Streaming](https://github.com/mavlink/qgroundcontrol/tree/master/src/VideoStreaming) directory for further instructions.