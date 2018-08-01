# Plan File Format

Plan files are stored in JSON file format and contain mission items and (optional) geo-fence and rally-points associated with the flight plan.
Below you can see the top level format of a Plan file

> **Tip** This is "near-minimal" - a plan must contain at least one mission item.

```json
{
    "fileType": "Plan",
    "geoFence": {
        "circles": [
        ],
        "polygons": [
        ],
        "version": 2
    },
    "groundStation": "QGroundControl",
    "mission": {
    },
    "rallyPoints": {
        "points": [
        ],
        "version": 2
    },
    "version": 1
}
```

The main fields are:

Key | Description
--- | ---
`fileType` | Must be `"Plan"`.
`version` | The version for this file. Current version is 1.
`groundStation` | The name of the ground station which created this file (here *QGroundControl*)
[`mission`](#mission) | The mission associated with this flight plan.
[`geoFence`](#geofence) | (Optional) Geofence information for this plan.
[`rallyPoints`](#rally_points) | (Optional) Rally/Safe point information for this plan


## Mission Object {#mission}

The structure of the mission object is shown below.
The `items` field contains a comma-separated list of mission items (it must contain at least one mission item, as shown below).
The list may be a mix of both [SimpleItem](#mission_simple_item) and [ComplexItem](#mission_complex_item) objects.

```json
    "mission": {
        "cruiseSpeed": 15,
        "firmwareType": 12,
        "hoverSpeed": 5,
        "items": [
            {
                "AMSLAltAboveTerrain": null,
                "Altitude": 50,
                "AltitudeMode": 0,
                "autoContinue": true,
                "command": 22,
                "doJumpId": 1,
                "frame": 3,
                "params": [
                    15,
                    0,
                    0,
                    null,
                    47.3985099,
                    8.5451002,
                    50
                ],
                "type": "SimpleItem"
            }
        ],
        "plannedHomePosition": [
            47.3977419,
            8.545594,
            487.989
        ],
        "vehicleType": 2,
        "version": 2
    },
```

The following values are required:

Key | Description
--- | ---
`version` | The version for the mission object. Current version is 2.
`firmwareType` | The firmware type for which this mission was created. This is one of the [MAV_AUTOPILOT](https://mavlink.io/en/messages/common.html#MAV_AUTOPILOT) enum values. 
`vehicleType` | The vehicle type for which this mission was created. This is one of the [MAV_TYPE](https://mavlink.io/en/messages/common.html#MAV_TYPE) enum values.
`cruiseSpeed` | The default forward speed for Fixed wing or VTOL vehicles (i.e. when moving between waypoints).
`hoverSpeed` | The default forward speed for multi-rotor vehicles.
`items` | The list of mission item objects associated with the mission . The list may contain either/both [SimpleItem](#mission_simple_item) and [ComplexItem](#mission_complex_item) objects.
`plannedHomePosition` | The planned home position is shown on the map and used for mission planning when no vehicle is connected. The array values shown above are (from top): latitude, longitude and AMSL altitude.

The format of the simple and complex items is given below.

### `SimpleItem` - Simple Mission Item {#mission_simple_item}

A simple item represents a single MAVLink [MISSION_ITEM](https://mavlink.io/en/messages/common.html#MISSION_ITEM) command.

```
            {
                "AMSLAltAboveTerrain": null,
                "Altitude": 50,
                "AltitudeMode": 0,
                "autoContinue": true,
                "command": 22,
                "doJumpId": 1,
                "frame": 3,
                "params": [
                    15,
                    0,
                    0,
                    null,
                    47.3985099,
                    8.5451002,
                    50
                ],
                "type": "SimpleItem"
            }
```

The field mapping is shown below.

Key | Description
--- | ---
`type` | `SimpleItem` for a simple item
`AMSLAltAboveTerrain` | 
`Altitude` | 
`AltitudeMode` | 
`autoContinue` | [MISSION_ITEM](https://mavlink.io/en/messages/common.html#MISSION_ITEM).autoContinue
`command` | The command ([MAV_CMD](https://mavlink.io/en/messages/common.html#MAV_CMD)) for this mission item - see [MISSION_ITEM](https://mavlink.io/en/messages/common.html#MISSION_ITEM).command.
`doJumpId` | The target id for the current mission item in DO_JUMP commands. These are auto-numbered from 1.
`frame` | [MAV_FRAME](https://mavlink.io/en/messages/common.html#MAV_FRAME) (see [MISSION_ITEM](https://mavlink.io/en/messages/common.html#MISSION_ITEM).frame)
`params` | [MISSION_ITEM](https://mavlink.io/en/messages/common.html#MISSION_ITEM).param1,2,3,4,x,y,z (values depends on the particular [MAV_CMD](https://mavlink.io/en/messages/common.html#MAV_CMD)).


### `ComplexItem` - Complex Mission Item {#mission_complex_item}

A complex item is a higher level encapsulation of multiple MISSION_ITEMS treated as a single entity.

```
{
    "complexItemType": "survey",
    "type": "ComplexItem",
    "version": 3,
    ...
},
```

Complex items have these values associated with them:

Key | Description
--- | ---
`TransectStyleComplexItem` | ?
`type` | `ComplexItem` for a complex item
`complexItemType` | The type of complex item. *QGroundControl* currently supports the following types: [survey](../file_formats/survey.md), fwLandingPattern
`version` | The version of the complex item specification. Here we document version 4.
`angle` | 
`entryLocation` | 
`flyAlternateTransects` | 
`polygon` | 


<!-- Example kept here for  discussion: 

```json
            {
                "TransectStyleComplexItem": {
                    "CameraCalc": {
                        "AdjustedFootprintFrontal": 25,
                        "AdjustedFootprintSide": 25,
                        "CameraName": "Manual (no camera specs)",
                        "DistanceToSurface": 50,
                        "DistanceToSurfaceRelative": true,
                        "version": 1
                    },
                    "CameraTriggerInTurnAround": true,
                    "FollowTerrain": false,
                    "HoverAndCapture": false,
                    "Items": [
                        {
                            "autoContinue": true,
                            "command": 16,
                            "doJumpId": 7,
                            "frame": 3,
                            "params": [
                                0,
                                0,
                                0,
                                null,
                                47.39626293736067,
                                8.539767750127863,
                                50
                            ],
                            "type": "SimpleItem"
                        },
...
                        {
                            "autoContinue": true,
                            "command": 206,
                            "doJumpId": 132,
                            "frame": 2,
                            "params": [
                                0,
                                0,
                                0,
                                0,
                                0,
                                0,
                                0
                            ],
                            "type": "SimpleItem"
                        }
                    ],
                    "Refly90Degrees": false,
                    "TurnAroundDistance": 10,
                    "VisualTransectPoints": [
                        [
                            47.39626293736067,
                            8.539767750127863
                        ],
                        [
                            47.39808524022186,
                            8.542092722133443
                        ],
...
                        [
                            47.396701075690515,
                            8.54973188084544
                        ]
                    ],
                    "version": 1
                },
                "angle": 0,
                "complexItemType": "survey",
                "entryLocation": 0,
                "flyAlternateTransects": false,
                "polygon": [
                    [
                        47.39804864324021,
                        8.541388259588786
                    ],
                    [
                        47.39843354693733,
                        8.548801558656066
                    ],
                    [
                        47.39635285483925,
                        8.549863713415386
                    ],
                    [
                        47.39635285483925,
                        8.53943561141819
                    ]
                ],
                "type": "ComplexItem",
                "version": 4
            }
```

--> 

## GeoFence {#geofence}

Geofence information is optional. 
The plan can contain an arbitrary number of geofences defined in terms of polygons and circles.

The minimal definition is shown below. 

```json
    "geoFence": {
        "circles": [
        ],
        "polygons": [
        ],
        "version": 2
    },
```

The fields are:

Key | Description
--- | ---
`version` | The version number for the geofence plan format. The documented version is 2.
[`circles`](#circle_geofence) | List containing circle geofence definitions (comma separated).
[`polygons`](#polygon_geofence) | List containing polygon geofence definitions  (comma separated).


### Circle Geofence {#circle_geofence}

Each circular geofence is defined in a separate item, as shown below (multiple comma-separated items can be defined).
The items define the centre and radius of the circle, and whether or not the specific geofence is activated.

```json
            {
                "circle": {
                    "center": [
                        47.39756763610029,
                        8.544649762407738
                    ],
                    "radius": 319.85
                },
                "inclusion": true,
                "version": 1
            }
```

The fields are:

Key | Description
--- | ---
`version` | The version number for the geofence "circle" plan format. The documented version is 1.
`circle` | The definition of the circle. Includes `centre` (latitude, longitude) and `radisu` as shown above.
`inclusion` | Whether or not the geofence is enabled (true) or disabled.



### Polygon Geofence {#polygon_geofence}

Each polygon geofence is defined in a separate item, as shown below (multiple comma-separated items can be defined).
The geofence includes a set of points defined with a clockwise winding (i.e. they must enclose an area).

```json
            {
                "inclusion": true,
                "polygon": [
                    [
                        47.39807773798406,
                        8.543834631785785
                    ],
                    [
                        47.39983519888905,
                        8.550024648373267
                    ],
                    [
                        47.39641100087146,
                        8.54499282423751
                    ],
                    [
                        47.395590322265186,
                        8.539435808992085
                    ]
                ],
                "version": 1
            }
        ],
        "version": 2
    }
```

The fields are:

Key | Description
--- | ---
`version` | The version number for the geofence "polygon" plan format. The documented version is 2.
`polygon` | A list of points for the polygon. Each point contains a latitude and longitude. The points are ordered in a clockwise winding.
`inclusion` | Whether or not the geofence is enabled (true) or disabled.


## Rally Points {#rally_points}

Rally point information is optional. 
The plan can contain an arbitrary number of rally points, each of which has a latitude, longitude, and altitude (above home position).

A definition with two points is shown below.

```json

    "rallyPoints": {
        "points": [
            [
                47.39760401,
                8.5509154,
                50
            ],
            [
                47.39902017,
                8.54263274,
                50
            ]
        ],
        "version": 2
    }
```

The fields are:

Key | Description
--- | ---
`version` | The version number for the rally point plan format. The documented version is 2.
`points` | A list of rally points.