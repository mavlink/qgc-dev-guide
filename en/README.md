# QGroundControl Dev Guide

This guide explains how *QGroundControl* (QGC) works internally, and provides guidelines for contributing code to the project. It is intended for use by developers!

> **Tip** To learn how to **use** *QGroundControl*, see the [User Guide](https://donlakeflyer.gitbooks.io/qgroundcontrol-user-guide/content/).

&nbsp;
> **Note** This guide is an active work in progress. The information provided should be correct, but you may find missing information or incomplete pages.

## Design Philosophy

QGC is designed to provide a single codebase that can run across multiple OS platforms as well as multiple device sizes and styles.

The QGC user interface is implemented using [Qt QML](http://doc.qt.io/qt-5/qtqml-index.html). QML provides for hardware acceleration which is a key feature on lower powered devices such as tablets or phones. QML also provides features which allows us to more easily create a single user interface which can adapt itself to differing screen sizes and resolution.

The QGC UI targets itself more towards a tablet+touch style of UI than a desktop mouse-based UI. This make a single UI easier to create since tablet style UI also tends to work fine on desktop/laptops.
