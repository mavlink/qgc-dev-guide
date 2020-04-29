# Custom Builds

A custom build allows third parties to create their own version of QGC in a way that allows them to easily keep up to date with changes which are being made in regular QGC. QGC has an architecture built into it which allows customs builds to modify and add to the feature set of regular QGC.

This is done through the QGCCorePlugin.h, FirmwarePlugin.h and AutoPilotPlugin.h support. If you want to understand the possibilities, the first step is to read through those files which document what is possible.

The QGC build system has support for a custom build through the existence of a ```custom``` build directory at the root of the source tree. If this directory exists QGC will build a custom version of QGC applying the modifications from the code in the custom directory.

The regular QGC repo has an example of a custom build under the ```custom_example``` directory. To try it out you can build it by renaming ```custom_example``` to ```custom``` and building QGC using the normal steps.