# Unit Tests

QGC contains a set of unit tests which must pass before a change will be considered through a pull request. The addition of new complex subsystems to QGC should include a corresponding new unit test to test it.

You can run unit tests from the command line using the ```--unittest``` command line option which will runs all tests. You can run a specific unit test by specifying it: ```--unittest:RadioConfigTest```.

The full list of unit tests can be found in UnitTestList.cc.