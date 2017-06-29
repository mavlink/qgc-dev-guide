# Complex Item - Survey

A survey creates a flight path over a polygonal area in a mission.

> **Note** All values unless otherwise specified are in meters.

```
{
   "camera": {
        "focalLength": 16,
        "groundResolution": 3,
        "imageFrontalOverlap": 10,
        "imageSideOverlap": 10,
        "name": "Sony ILCE-QX1",
        "orientationLandscape": true,
        "resolutionHeight": 3632,
        "resolutionWidth": 5456,
        "sensorHeight": 15.4,
        "sensorWidth": 23.199999999999999
    },
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
            47.633933816132817,
            -122.08937942845
        ],
        [
            47.634139864633021,
            -122.08781838280333
        ],
        [
            47.633395194285789,
            -122.08872496945037
        ]
    ],
    "type": "ComplexItem",
    "version": 3
}
```

A Survey is represented as a JSON object which is stored within the ```items``` array. It stores all the meta data associated with a Survey. It does not store the individual waypoints within the Survey. Those are generated when the mission is loaded by QGroundControl. 

Key | Description
----|------------
camera | Object which specifies the values associated with the camera being used for the survey. Only required if `manualGrid` is `false`.
cameraTrigger | Specifies whether the camera should be triggered at the specified `cameraTriggerDistance` intervals.
complexItemType | Identifies this complex mission item as a `survey`.
fixedValueIsAltitude | Specifies whether Altitude should be kept constant when modifying other values from the Survey user interface. Only used by the QGroundControl UI.
grid | Object which specifies the values associated with the survey grid.
manualGrid | `true`: Grid values were specified manually by the user, `false`: Grid values are based on camera settings specified by the `camera` object.
polygon | The polygon array which represents the polygonal survey area. Each point is a latitude, longitude pair for a polygon vertex.
type | Specifies that this item is a `ComplexItem`.
version | Version number for the Survey Complex Mission Item format. Current version is 3.


## Grid Object

The `grid` object specifies the values associated with the survey grid.

```
"grid": {
    "altitude": 50,
    "angle": 0,
    "relativeAltitude": true,
    "spacing": 30,
    "turnAroundDistance": 0
},
```

Key | Description
----|------------
altitude | The altitude for all transect waypoints within the grid.
angle | The angle in degrees for the transect paths.
relativeAltitude | `true`: `altitude` is relative to home, `false`: `altitude` is AMSL
spacing | The spacing in between each transect.
turnAroundDistance | The distance to fly past the polygon edge prior to turning for the next transect.


## Camera Object

The `camera` object specifies the values associated with the camera being used for the survey. This object is only required if `manualGrid` is `false`.

```
"camera": {
    "focalLength": 16,
    "groundResolution": 3,
    "imageFrontalOverlap": 10,
    "imageSideOverlap": 10,
    "name": "Sony ILCE-QX1",
    "orientationLandscape": true,
    "resolutionHeight": 3632,
    "resolutionWidth": 5456,
    "sensorHeight": 15.4,
    "sensorWidth": 23.199999999999999
},
```

Key | Description
----|------------
focalLength | Focal length of camera lens in millimeters.
groundResolution | Target ground resolution in cm/px.
imageFrontalOverlap | Percentage of frontal image overlap.
imageSideOverlap | Percentage of side image overlap.
name | Name of camera being used. Should correspond to one of the cameras known to QGroundControl. Use `"Custom Camera Grid"` for custom camera specifications,
orientationLandscape | `true`: Camera installed in landscape orientation on vehicle, `false`: Camera installed in portrait orientation on vehicle
resolutionHeight, resolutionWidth | Image pixel resolution.
sensorHeight, sensorWidth | Sensor dimensions in millimeters.