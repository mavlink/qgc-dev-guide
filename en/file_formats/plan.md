# Plan File Format
> **Note** All values unless otherwise specified are in meters.

```
{
    "fileType": "Plan",
    "version": 1
    "groundStation": "QGroundControl",
    "mission": {
        "version": 2
        "firmwareType": 12,
        "vehicleType": 2,
        "cruiseSpeed": 15,
        "hoverSpeed": 5,
        "plannedHomePosition": [
            47.632939716176864,
            -122.08905141,
            40
        ],
        "items": [
     	    ...
        ],
    },
    "geoFence": {
        ...
    },
    "rallyPoints": {
        ...
    },
}
```

Above you can see the top level format of a Plan file. Plan files are stored in JSON file format and contain the mission, geo-fence and rally-points associated with the flight plan.

Key | Description
----|------------
fileType | Must be "Plan".
version | The version for this file. Current version is 1.
groundStation | The name of the ground station which created this file.
mission | The mission associated with this flight plan.
geoFence | (Optional)
rallyPoints | (Optional)

## Mission Object

The following values are required:

Key | Description
----|------------
version | The version for the mission object. Current version is 2.
firmwareType | The firmware type that this mission was created for using the MAVLink MAV_AUTOPILOT enum values. 
vehicleType | The vehicle type that this mission was created for using the MAVLink MAV_TYPE enum values.
cruiseSpeed | 
hoverSpeed | 
items | The list of mission item objects associated with the mission.
plannedHomePosition | The planned home position to show on the map when you are editing the mission. Values with array are latitude, longitude and altitude.

## Mission Item - `SimpleItem`
A simple item represents a single MAVLink [MISSION_ITEM](http://mavlink.org/messages/common#MISSION_ITEM) command.

```
{
    "autoContinue": true,
    "command": 22,
    "frame": 2,
    "params": [
        0,
        0,
        0,
        0,
        47.633127690000002,
        -122.08867133,
        50
    ],
    "type": "SimpleItem"
},
```

The values in a `SimpleItem` map directly to the values in [MISSION_ITEM](http://mavlink.org/messages/common#MISSION_ITEM):

Key | Description
----|------------
autoContinue | MISSION_ITEM.autoContinue
command | MISSION_ITEM.command
frame | MISSION_ITEM.frame
params | MISSION_ITEM.param1,2,3,4,x,y,z

## Mission Item - ```ComplexItem```
A complex item is a higher level encapsulation of multiple MISSION_ITEMS treated as a single entity.

```
{
    "complexItemType": "survey",
    "type": "ComplexItem",
    "version": 3,
    ...
},
```
Complex items have two additional values associated with them:

Key | Description
----|------------
complexItemType | Specifies the type of complex item. QGroundControl currently supports the following types: [survey](../file_formats/survey.md), [fwLandingPattern](../file_formats/fwLandingPattern.md)
version | Specifies the version for this complex item.

## Special handling for DO_JUMP mission item
Since DO_JUMP command requires you to specify the sequence number to jump to and the mission file format does not specify sequence numbers it require special handling.

First you must assign a unique identifier to the mission item you want to jump to:

```
{
    ...
    "doJumpId": 100
}
```

The `doJumpId` can be any value greater than 0 and must uniquely identify a DO_JUMP target.

Then in the actual DO_JUMP mission item you reference this unique id in the MISSION_ITEM.param1 value:

```
{
    ...
    "command": 177,
    "params": [
        100,
        ...
    ],
    ...
},
```

When the mission is loaded the actual DO_JUMP sequence number will be determined and filled in.