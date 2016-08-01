# QGroundControl Dev Guide

This guide is meant for use by developers who want to understand more deeply how the internals of QGC work. It also provides guidelines for developers who want to contribute pull requests to the project. It is not meant to explain how to use QGroundControl, that information is in the [user guide](https://donlakeflyer.gitbooks.io/qgroundcontrol-user-guide/content/).

## Design Philosophy
From a codebase standpoint QGC is designed to provide a single codebase which will run across multiple OS platforms as well as multiple device sizes and styles.

The user interface of QGC is implemented using Qml. Qml provides for hardware acceleration which is a key feature on lower powered devices such as tablets or phones. Qml also provides features which allows us to more easily create a single user interface which can adapt itself to differening screen sizes and resolution.

**Note: The QGroundControl Dev Guide is an active work in progress. The information provided should be correct, but you may find missing information or incomplete pages.**