# QGroundControl Dev Guide

[![Releases](https://img.shields.io/github/release/mavlink/QGroundControl.svg)](https://github.com/mavlink/QGroundControl/releases) [![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/mavlink/qgroundcontrol?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) [![Slack](https://px4-slack.herokuapp.com/badge.svg)](http://slack.px4.io) [![Discuss](https://img.shields.io/badge/discuss-dev-ff69b4.svg)](http://discuss.px4.io/c/qgroundcontrol/qgroundcontrol-developers)

This developer guide is the best source for information if you want to build, modify or extend [QGroundControl](http://qgroundcontrol.com) (QGC). 
It shows how to obtain and build the source code, explains how QGC works, and provides guidelines for contributing code to the project. 

> **Tip** This guide is for **developers**! To learn how to **use** *QGroundControl*, see the [User Guide](https://docs.qgroundcontrol.com/en/).

<span></span>
> **Note** This guide is an active work in progress - information should be correct, but may not be complete!
> If you find that it is missing helpful information (or errors) please raise an [issue](https://github.com/mavlink/qgc-dev-guide/issues).

## Design Philosophy

QGC is designed to provide a single codebase that can run across multiple OS platforms as well as multiple device sizes and styles.

The QGC user interface is implemented using [Qt QML](http://doc.qt.io/qt-5/qtqml-index.html). QML provides for hardware acceleration which is a key feature on lower powered devices such as tablets or phones. QML also provides features which allows us to more easily create a single user interface which can adapt itself to differing screen sizes and resolution.

The QGC UI targets itself more towards a tablet+touch style of UI than a desktop mouse-based UI. This make a single UI easier to create since tablet style UI also tends to work fine on desktop/laptops.

## Support

Development questions can be raised in the [QGroundControl Developer](http://discuss.px4.io/c/qgroundcontrol/qgroundcontrol-developers) discuss category or in the *QGroundControl* [Gitter](https://gitter.im/mavlink/qgroundcontrol) channel. 


## License

*QGroundControl* source code is [dual-licensed under Apache 2.0 and GPLv3](https://github.com/mavlink/qgroundcontrol/blob/master/COPYING.md).

