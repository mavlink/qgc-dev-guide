# MAVLink Customisation

QGC communicates with flight stacks using [MAVLink](https://mavlink.io/en/), a very lightweight messaging protocol that has been designed for the drone ecosystem.
QGC includes the [ArduPilotMega.xml](https://mavlink.io/en/messages/ardupilotmega.html) dialect by default, which allows it to communicate with both PX4 and Ardupilot (PX4 uses [common.xml](https://mavlink.io/en/messages/common.html), which is incuded in ArduPilotMega).

In order to add support for a new set of messages you will ultimately need to add them to `ArduPilotMega.xml` or `common.xml`, or fork *QGroundControl* and include your own dialect.

To do this:
- Replace the pre-build C library at [/qgroundcontrol/libs/mavlink/include/mavlink](https://github.com/mavlink/qgroundcontrol/tree/master/libs/mavlink/include/mavlink).
  - By default this is a submodule importing https://github.com/mavlink/c_library_v2
  - You can change the submodule, or [build your own libraries](https://mavlink.io/en/getting_started/generate_libraries.html) using the MAVLink toolchain.
- You can change the whole dialect used by setting it in [`MAVLINK_CONF`](https://github.com/mavlink/qgroundcontrol/blob/master/QGCExternalLibs.pri#L52) when running *qmake*.