# Getting Started

This topic explains how to get the *QGroundControl* source code and build it either natively or within a *Vagrant* environment. 
It also provides information about optional or OS specific functionality.

## Daily Builds

If you just want to test (and not debug) a recent build of *QGroundControl* you can use the [Daily Build](https://docs.qgroundcontrol.com/en/releases/daily_builds.html). Versions are provided for all platforms. 

## Source Code

Source code for *QGroundControl* is kept on GitHub here: https://github.com/mavlink/qgroundcontrol.
It is [dual-licensed under Apache 2.0 and GPLv3](https://github.com/mavlink/qgroundcontrol/blob/master/COPYING.md).

To get the source files:
1. Clone the repo (or your fork) including submodules:
   ```
   git clone https://github.com/mavlink/qgroundcontrol.git --recursive
   ```
2. Update submodules (required each time you pull new source code):
   ```
   git submodule update
   ```

> **Tip** Github source-code zip files cannot be used because these do not contain the appropriate submodule source code. You must use git!


## Build QGroundControl

### Native Builds

*QGroundControl* builds are supported for macOS, Linux, Windows, iOS and Android. *QGroundControl* uses [Qt](http://www.qt.io) as its cross-platform support library and uses [QtCreator](http://doc.qt.io/qtcreator/index.html) as its default build environment.

- **macOS:** v10.11 or higher
- **Ubuntu:** 64 bit, gcc compiler
- **Windows:** Vista or higher, [Visual Studio 2015 compiler](#vs2015) (32 bit)
- **iOS:** 10.0 and higher
- **Android:** Jelly Bean (4.1) and higher. Standard QGC is built against ndk version 19.
- **Qt version:** {{ book.qt_version }} **(only)** (except for Ubuntu, which uses Qt 5.11.3) <!-- NOTE {{ book.qt_version }} is set in the variables section of gitbook file https://github.com/mavlink/qgc-dev-guide/blob/master/book.json -->

> **Tip** For more information see: [Qt 5 supported platform list](http://doc.qt.io/qt-5/supported-platforms.html).


#### Install Visual Studio 2015 (Windows Only) {#vs2015}

The Windows compiler can be found here: [Visual Studio 2015 compiler](https://visualstudio.microsoft.com/vs/older-downloads/) (32 bit)

When installing, you must minimally select all Visual C++ components as shown:
![Visual Studio 2015 - Select all Visual C++ Components](../../assets/getting_started/vs_2015_select_features.png)


#### Install Qt

You **need to install Qt as described below** instead of using pre-built packages from say, a Linux distribution, because *QGroundControl* needs access to private Qt headers.

To install Qt:
1. Download and run the [Qt Online Installer](http://www.qt.io/download-open-source)
   - **Ubuntu:** 
     - Set the downloaded file to executable using: `chmod +x`. 
     - Install to default location for use with **./qgroundcontrol-start.sh.** If you install Qt to a non-default location you will need to modify **qgroundcontrol-start.sh** in order to run downloaded builds.
1. In the installer *Select Components* dialog choose: {{ book.qt_version }} (on *Ubuntu* choose Qt 5.11.3).
   
   Then install just the following components: 
   - **Windows**: *MCVC 2015 32 bit*
   - **MacOS**: *macOS*
   - **Linux**: *Desktop gcc 64-bit*
   - All:
     - *Qt Charts* and *Qt Remote Objects (TP)*
     - *Android ARMv7* (to build Android)
1. Install Additional Packages (Platform Specific)
   - **Ubuntu:** `sudo apt-get install speech-dispatcher libudev-dev libsdl2-dev`
   - **Fedora:** `sudo dnf install speech-dispatcher SDL2-devel SDL2 systemd-devel`
   - **Arch Linux:** `pacman -Sy speech-dispatcher`
   - **Windows:** [USB Driver](http://www.pixhawk.org/firmware/downloads) to connect to Pixhawk/PX4Flow/3DR Radio
   - **Android:** [Qt Android Setup](http://doc.qt.io/qt-5/androidgs.html)

#### Building using Qt Creator

1. Launch *Qt Creator* and open the **qgroundcontrol.pro** project.
1. Select the appropriate kit for your needs:
  - **OSX:** Desktop Qt {{ book.qt_version }} clang 64 bit
    > **Note** iOS builds must be built using [XCode](http://doc.qt.io/qt-5/ios-support.html).
  - **Ubuntu:** Desktop Qt 5.11.3 <!-- {{ book.qt_version }} --> GCC 64bit
  - **Windows:** Desktop Qt {{ book.qt_version }} MSVC2015 **32bit**
  - **Android:** Android for armeabi-v7a (GCC 4.9, Qt {{ book.qt_version }})
1. Build using the "hammer" (or "play") icons:
   
   ![QtCreator Build Button](../../assets/getting_started/qt_creator_build_qgc.png)


### Vagrant

[Vagrant](https://www.vagrantup.com/) can be used to build and run *QGroundControl* within a Linux virtual machine (the build can also be run on the host machine if it is compatible).

1. [Download](https://www.vagrantup.com/downloads.html) and [Install](https://www.vagrantup.com/docs/getting-started/) Vagrant
1. From the root directory of the *QGroundControl* repository run `vagrant up`
1. To use the graphical environment run `vagrant reload`

### Additional Build Notes for all Supported OS

* **Warnings as Errors:** Specifying `CONFIG+=WarningsAsErrorsOn` will turn all warnings into errors which breaks the build. If you are working on a pull request you plan to submit to github for consideration, you should always run with this setting turned on, since it is required for all pull requests. 
  > **Note** Putting this line into a file called **user_config.pri** in the top-level directory (same directory as **qgroundcontrol.pro**) will set this flag on all builds without interfering with the GIT history.
* **Parallel builds:** For non Windows builds, you can use the `-j#` option to run parellel builds.
* **Location of built files:** Individual build file results can be found in the `build_debug` or `build_release` directories. The built executable can be found in the `debug` or `release` directory.
* **If you get this error when running _QGroundControl_**: `/usr/lib/x86_64-linux-gnu/libstdc++.so.6: version 'GLIBCXX_3.4.20' not found.`, you need to either update to the latest *gcc*, or install the latest *libstdc++.6* using: `sudo apt-get install libstdc++6`.
* **Unit tests:** To run the [unit tests](../contribute/unit_tests.md), build in `debug` mode with `UNITTEST_BUILD` definition, and then copy `deploy/qgroundcontrol-start.sh` script into the `debug` directory before running the tests.

## Optional/OS-Specific Functionality

*QGroundControl* has functionality that is dependent on the operating system and libraries installed by the user. The following sections describe these features, their dependencies, and how to disable/alter them during the build process. These features can be forcibly enabled/disabled by specifying additional values to qmake. 

### Opal-RT's RT-LAB Simulator

Integration with Opal-RT's RT-LAB simulator can be enabled on Windows by installing RT-LAB 7.2.4. This allows vehicles to be simulated in RT-LAB and communicate directly with QGC on the same computer as if the UAS was actually deployed. This support is enabled by default once the requisite RT-LAB software is installed. Disabling this can be done by adding `DEFINES+=DISABLE_RTLAB` to qmake.

### XBee Support

*QGroundControl* can talk to XBee wireless devices using their proprietary protocol directly on Windows and Linux platforms. This support is not necessary if you're not using XBee devices or aren't using their proprietary protocol. On Windows, the necessary dependencies are included in this repository and no additional steps are required. For Linux, change to the `libs/thirdParty/libxbee` folder and run `make;sudo make install` to install libxbee on your system (uninstalling can be done with a `sudo make uninstall`). *qmake* will automatically detect the library on Linux, so no other work is necessary.

To disable XBee support you may add `DEFINES+=DISABLE_XBEE` to *qmake*.

### Video Streaming

Check the [Video Streaming](https://github.com/mavlink/qgroundcontrol/tree/master/src/VideoStreaming) directory for further instructions.

