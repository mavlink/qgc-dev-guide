# User Interface Design

The main pattern for UI design in QGC is a UI page written in Qml, many times communicated with a custom "Controller" written in C++. This follows a somewhat hacked variant of the MVC design pattern. 

The Qml code binds to information associated with the system through the following mechanisms:
* The custom controller
* The global QGroundControl object which provides access to things like the active Vehicle
* The FactSystem which provides access to parameters and in some cases custom Facts.

Note: Due to the complexity of the Qml used in QGC as well as it's reliance on communication with C++ objects to drive the ui it is not possible to use the Qml Designer provided by Qt to edit Qml.