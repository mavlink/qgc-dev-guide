# Communication Flow

Description of the high level communication flow which takes place during a vehicle auto-connect.

* LinkManager always has a UDP link open waiting for a Vehicle heartbeat
* LinkManager detects a new known device (Pixhawk, SiK Radio, PX4 Flow) connected to computer
    * Creates a new SerialLink connected to the device
* Bytes comes through Link and are sent to MAVLinkProtocol
* MAVLinkProtocol converts the bytes into a MAVLink message
* If the message is a `HEARTBEAT` the MultiVehicleManager is notified
* MultiVehicleManager is notifed of the ```HEARTBEAT``` and creates a new Vehicle object based on the information in the `HEARTBEAT` message
* The Vehicle instantiates the plugins which match the vehicle type
* The ParameterLoader associated with the vehicle sends a ```PARAM_REQUEST_LIST``` to the vehicle to load params using the parameter protocol
* Once parameter load is complete, the MissionManager associated with the Vehicle requests the mission items from the Vehicle using the mission item protocol
* Once parameter load is complete, the VehicleComponents display their UI in the Setup view