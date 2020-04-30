# 自定义构建

自定义构建允许第三方创建自己的QGC版本，使其能够轻松跟上常规QGC中所做的更改。 QGC内置了一个架构，允许用户对构建进行定制，实现对常规QGC功能集的修改和扩充。

One of the downsides of QGC providing both generic support for any vehicle which supports mavlink as well as providing firmware specific support for both PX4 Pro and ArduPilot is complexity of the user interface. Since QGC doesn't know any information about your vehicle ahead of time it requires UI bits which can get in they way if say the vehicle you fly only uses PX4 Pro firmware and is a multi-rotor vehicle. If that is a known thing then the UI can be simplified in various places. Also QGC targets both DIY users who are building their own vehicles from scratch as well as commercial users of off the shelf vehicles. Setting up a DIY drone from scratch requires all sort of functionality which is not needed for users of off the shelf vehicles. So for off the shelf vehicle users all the DIY specific stuff is just extra noise they need to look past. Creating a custom build allows you to specify exact details for your vehicle and hide things which are irrelevant thus creating and even simple user experience for your users than regular generic QGC.

This is done through the QGCCorePlugin.h, FirmwarePlugin.h and AutoPilotPlugin.h support. If you want to understand the possibilities, the first step is to read through those files which document what is possible.

There is also a mechanism which allows you to overrides resources so you can change the smaller visual elements in QGC.

The QGC build system has support for a custom build through the existence of a ```custom``` build directory at the root of the source tree. If this directory exists QGC will build a custom version of QGC applying the modifications from the code in the custom directory. The goal of custom build development is to keep all of your changes below this custom directory. Anything above here is regular QGC and if you change that you could lose your merge ability to pull new features from upstream or at least make that process a royal PITA. This goal isn't always possible, but the less you hack around in regular QGC code thus truly "forking" QGC the better off you will be.

The regular QGC repo has an example of a custom build under the ```custom_example``` directory. To try it out you can build it by renaming ```custom_example``` to ```custom``` and building QGC using the normal steps.