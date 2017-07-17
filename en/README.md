# QGroundControl Dev Guide

[![Releases](https://img.shields.io/github/release/mavlink/QGroundControl.svg)](https://github.com/mavlink/QGroundControl/releases) [![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/mavlink/qgroundcontrol?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) [![Slack](https://px4-slack.herokuapp.com/badge.svg)](http://slack.px4.io) [![Discuss](https://img.shields.io/badge/discuss-dev-ff69b4.svg)](http://discuss.px4.io/c/qgroundcontrol/qgroundcontrol-developers)


This guide explains how *QGroundControl* (QGC) works internally, and provides guidelines for contributing code to the project. It is intended for use by developers!

> **Tip** To learn how to **use** *QGroundControl*, see the [User Guide](https://docs.qgroundcontrol.com/en/).

<span></span>
> **Note** This guide is an active work in progress. The information provided should be correct, but you may find missing information or incomplete pages.


## Support

This developer guide will eventually be the main provider of information about *QGroundControl* development. If you find that it is missing helpful information and/or has incorrect information please raise an [issue](https://github.com/mavlink/qgc-dev-guide/issues).

Development questions can be raised in the [QGroundControl Developer](http://discuss.px4.io/c/qgroundcontrol/qgroundcontrol-developers) discuss category or in the *QGroundControl* [Gitter](https://gitter.im/mavlink/qgroundcontrol) channel. 

## Design Philosophy

QGC is designed to provide a single codebase that can run across multiple OS platforms as well as multiple device sizes and styles.

The QGC user interface is implemented using [Qt QML](http://doc.qt.io/qt-5/qtqml-index.html). QML provides for hardware acceleration which is a key feature on lower powered devices such as tablets or phones. QML also provides features which allows us to more easily create a single user interface which can adapt itself to differing screen sizes and resolution.

The QGC UI targets itself more towards a tablet+touch style of UI than a desktop mouse-based UI. This make a single UI easier to create since tablet style UI also tends to work fine on desktop/laptops.
