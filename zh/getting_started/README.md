# 初始步骤

本主题说明如何获取QGroundControl源代码并在本机或在Vagrant(虚拟机)环境中构建它。 本主题还提供其他可选功能信息及特定于操作系统的功能信息。

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
* **Windows:**vista 或更高版本, < 1>Visual studio 2015 编译器 </1 > (32位)
* iOS：10.0及更高版本
* Android：Jelly Bean（4.1）及更高版本。 标准QGC是针对ndk版本19构建的。 标准QGC是针对ndk版本19构建的。
* ** Qt版本：</ 0> {{book.qt_version}} **（仅限）</ 0> <!-- NOTE {{ book.qt_version }} is set in the variables section of gitbook file https://github.com/mavlink/qgc-dev-guide/blob/master/book.json --></li> </ul> 
    
    > 提示: 有关更多信息，请参阅：Qt 5支持的平台列表。
    
    #### 安装 visual studio 2015 (仅限 windows) {#vs2015}
    
    windows 编译器可以在这里找到: Visual studio 2015 编译器 </0 > (32位)</p> 
    
    安装时, 必须选择的 visual c++ 组件, 如下所示: ![Visual Studio 2015 - Select all Visual C++ Components](../../assets/getting_started/vs_2015_select_features.png)
    
    #### 安装Qt
    
    请** 按照下面的方式安装 QT**, 而不是使用 linux 发行版中的预构建包, 因为 *QGroundControl* 需要访问专用 Qt标头。
    
    如何安装Qt：
    
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
                1. Install Additional Packages (Platform Specific)
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
            
            ![QtCreator Build Button](../../assets/getting_started/qt_creator_build_qgc.png)
        
        ### Vagrant
        
        [Vagrant](https://www.vagrantup.com/)可用于在Linux虚拟机中构建和运行QGroundControl（如果兼容，则构建也可以在主机上运行）
        
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
        
        *QGroundControl*的功能依赖于用户安装的操作系统和库。 以下部分描述了这些功能，它们的依赖关系，以及如何在构建过程中禁用/更改它们。 通过为qmake指定其他值，可以强制启用/禁用这些功能。
        
        ### Opal-RT的RT-LAB模拟器
        
        通过安装RT-LAB 7.2.4，可以在Windows上启用与Opal-RT的RT-LAB模拟器的集成。 这允许在RT-LAB中模拟车辆并在同一计算机上直接与QGC通信，就像实际部署UAS一样。 一旦安装了必需的RT-LAB软件，默认情况下将启用此支持。 可以通过向qmake添加`DEFINES + = DISABLE_RTLAB`来禁用此功能。
        
        ### XBee支持
        
        *QGroundControl*可以直接在Windows和Linux平台上通过其专属协议与XBee无线设备通信 如果您不使用XBee设备或未使用其专有协议，则无需此支持。 在Windows上，必需的依赖项包含在此存储库中，无需其他步骤。 对于Linux，进入`libs / thirdParty / libxbee`目录下，并运行`make; sudo make install`安装libxbee（如需卸载，请运行`sudo make uninstall`）。 *qmake* 将在 linux 上自动检测库文件, 因此无需用户进行其他操作。
        
        To disable XBee support you may add `DEFINES+=DISABLE_XBEE` to *qmake*.
        
        ### 视频流 
        
        Check the [Video Streaming](https://github.com/mavlink/qgroundcontrol/tree/master/src/VideoStreaming) directory for further instructions.
        
        ## 下载最新的开发版本
        
        QGroundControl mantains this download links to allow test and usage of the last updates in main code.
        
        * [Windows (QGroundControl-installer.exe)](https://s3-us-west-2.amazonaws.com/qgroundcontrol/builds/master/QGroundControl-installer.exe)
        * [MAC OS (QGroundControl.dmg)](https://s3-us-west-2.amazonaws.com/qgroundcontrol/builds/master/QGroundControl.dmg)
        * [Linux（QGroundControl.AppImage）](https://s3-us-west-2.amazonaws.com/qgroundcontrol/builds/master/QGroundControl.AppImage)
        * [Android (QGroundControl.apk)](https://s3-us-west-2.amazonaws.com/qgroundcontrol/builds/master/QGroundControl.apk)