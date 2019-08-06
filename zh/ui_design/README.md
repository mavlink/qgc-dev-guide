# 用户界面设计

The main pattern for UI design in QGC is a UI page written in QML, many times communicating with a custom "Controller" written in C++. 这是一个有点被黑客攻击的MVC设计模式。

QML代码通过以下机制绑定到与系统关联的信息：

* 自定义控制器
* 全局QGroundControl对象，提供对活动Vehicle等内容的访问
* FactSystem提供对参数的访问，在某些情况下提供自定义事实。

注意：由于QGC中使用的QML的复杂性以及它依赖于与C ++对象的通信来驱动ui，因此无法使用Qt提供的QML Designer来编辑QML。