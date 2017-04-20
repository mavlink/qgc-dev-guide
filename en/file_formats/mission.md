# Mission File Format (v2)
Note: All values unless otherwise specified are in meters.

```
{
    "firmwareType": 3,
    "groundStation": "QGroundControl",
    "items": [ 
        ... 
    ],
    "plannedHomePosition": [
        47.633033699999999,
        -122.0879364,
        3.5
    ],
    "version": 2
}
```

Above you can see the top level format of a mission file. Mission files are stored in JSON file format.

The following values are required:

* `firmwareType` The firmware type that this mission was created for using the `MAV_AUTOPILOT_*` MAVLink enum values. This allows you to distinguish between a mission which was created for PX4 or ArduPilot firmware. This can be important since each firmware handles mission command slightly differently.
* `groundStation` The name of the ground station which created this file.
* `items` The list of mission item objects associated with this mission. The format of these is described below.
* `plannedHomePosition` The planned home position to show on the map when you are editing this mission. Values with array are latitude, longitude and altitude.
* `version` The version for this file. Current version is 2.

## Mission Items
The ```items`` values contains an array of mission item objects. 

```
{
    "type": "SimpleItem",
    ...
},
```
The `type` value within a mission item specifies the type of the item. There are two types of mission items: `SimpleItem` and `ComplexItem`.

## Mission Item - `SimpleItem`
A simple item represents a single MAVLink [MISSION_ITEM](http://mavlink.org/messages/common#MISSION_ITEM). It contains all the values needed to store a [MISSION_ITEM](http://mavlink.org/messages/common#MISSION_ITEM).

```
{
    "autoContinue": true,
    "command": 22,
    "coordinate": [
        47.633127690000002,
        -122.08867133,
        50
    ],
    "frame": 2,
    "params": [
        0,
        0,
        0,
        0
    ],
    "type": "SimpleItem"
},
```

The values in a `SimpleItem` map directly to the values in [MISSION_ITEM](http://mavlink.org/messages/common#MISSION_ITEM):

* ```autoContinue``` MISSION_ITEM.autoContinue
* ```command``` MISSION_ITEM.command
* ```coordinate``` MISSION_ITEM.x-y-z
* ```frame``` MISSION_ITEM.frame
* ```params``` MISSION_ITEM.param1-2-3-4

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

* ```complexItemType``` Specifies the type of complex item. QGroundControl currently supports only one complex item type: [Survey](SurveyFileFormat.md)
* ```version``` Specifies the version for this complex item.

## Special handling for DO_JUMP mission item
Since DO_JUMP command requires you to specify the sequence number to jump to and the mission file format does not specify sequence numbers it require special handling.

First you must assign an identifier to the mission item you want to jump to:

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
        1,
        0,
        0
    ],
    ...
},
```

When the mission is loaded the actual DO_JUMP sequence number will be determined and filled in.

## Example mission file with Survey

```
{
    "firmwareType": 3,
    "groundStation": "QGroundControl",
    "items": [
        {
            "autoContinue": true,
            "command": 22,
            "coordinate": [
                47.633127690000002,
                -122.08867133,
                50
            ],
            "frame": 2,
            "params": [
                0,
                0,
                0,
                0
            ],
            "type": "SimpleItem"
        },
        {
            "autoContinue": true,
            "command": 16,
            "coordinate": [
                47.633178299999997,
                -122.08882689000001,
                50
            ],
            "frame": 3,
            "params": [
                0,
                0,
                0,
                0
            ],
            "type": "SimpleItem"
        },
        {
            "cameraTrigger": true,
            "cameraTriggerDistance": 25,
            "complexItemType": "survey",
            "fixedValueIsAltitude": false,
            "grid": {
                "altitude": 50,
                "angle": 0,
                "relativeAltitude": true,
                "spacing": 30,
                "turnAroundDistance": 0
            },
            "manualGrid": true,
            "polygon": [
                [
                    47.634139864633021,
                    -122.08895027500762
                ],
                [
                    47.634125405115633,
                    -122.08720683914794
                ],
                [
                    47.633561480817576,
                    -122.08859622341765
                ]
            ],
            "type": "ComplexItem",
            "version": 3
        },
        {
            "autoContinue": true,
            "command": 16,
            "coordinate": [
                47.633413269999998,
                -122.08736241,
                50
            ],
            "frame": 3,
            "params": [
                0,
                0,
                0,
                0
            ],
            "type": "SimpleItem"
        }
    ],
    "plannedHomePosition": [
        47.633033699999999,
        -122.0879364,
        3.5
    ],
    "version": 2
}
```

## Example mission file with DO_JUMP command
```
{
    "firmwareType": 3,
    "groundStation": "QGroundControl",
    "items": [
        {
            "autoContinue": true,
            "command": 22,
            "coordinate": [
                47.633131300000002,
                -122.08913267,
                50
            ],
            "frame": 2,
            "params": [
                0,
                0,
                0,
                0
            ],
            "type": "SimpleItem"
        },
        {
            "autoContinue": true,
            "command": 16,
            "coordinate": [
                47.633503640000001,
                -122.08926141000001,
                50
            ],
            "doJumpId": 2,
            "frame": 3,
            "params": [
                0,
                0,
                0,
                0
            ],
            "type": "SimpleItem"
        },
        {
            "autoContinue": true,
            "command": 16,
            "coordinate": [
                47.633951889999999,
                -122.08846748000001,
                50
            ],
            "frame": 3,
            "params": [
                0,
                0,
                0,
                0
            ],
            "type": "SimpleItem"
        },
        {
            "autoContinue": true,
            "command": 177,
            "coordinate": [
                47.633875979999999,
                -122.08738386,
                50
            ],
            "frame": 2,
            "params": [
                2,
                10,
                0,
                0
            ],
            "type": "SimpleItem"
        },
        {
            "autoContinue": true,
            "command": 16,
            "coordinate": [
                47.633293979999998,
                -122.08716391999999,
                50
            ],
            "frame": 3,
            "params": [
                0,
                0,
                0,
                0
            ],
            "type": "SimpleItem"
        }
    ],
    "plannedHomePosition": [
        47.633033699999999,
        -122.0879364,
        3.5
    ],
    "version": 2
}
```
