# 任务指令树

QGC 创建用户界面，用于从 json 元数据的层次结构中动态编辑特定任务项命令。 此层次结构称为任务命令树。 这样，在添加新命令时，只能创建 json 元数据。

## 为什么是一颗树？

需要该树以不同的方式处理不同固件和／或不同的车辆类型，以支持不同的命令。 最简单的例子是 mavlink 规范可能包含了并非所有固件都支持的命令参数。 或着命令参数仅对某些车辆类型有效。 此外，在某些情况下，GCS 可能会决定将某些命令参数在视图中对最终用户进行隐藏，因为它们过于复杂或导致可用性问题。

该树是MissionCommandTree类： [MissionCommandTree.cc](https://github.com/mavlink/qgroundcontrol/blob/master/src/MissionManager/MissionCommandTree.cc), [MissionCommandTree.h](https://github.com/mavlink/qgroundcontrol/blob/master/src/MissionManager/MissionCommandTree.h)

### 树根目录

树的根目录是与 mavlink 规范完全匹配的json元数据。

您可以在这里看到`MAV_CMD_NAV_WAYPOINT`根目录[json](https://github.com/mavlink/qgroundcontrol/blob/master/src/MissionManager/MavCmdInfoCommon.json#L27)的示例：

```
        {
            "id":                   16,
            "rawName":              "MAV_CMD_NAV_WAYPOINT",
            "friendlyName":         "Waypoint",
            "description":          "Travel to a position in 3D space.",
            "specifiesCoordinate":  true,
            "friendlyEdit":         true,
            "category":             "Basic",
            "param1": {
                "label":            "Hold",
                "units":            "secs",
                "default":          0,
                "decimalPlaces":    0
            },
            "param2": {
                "label":            "Acceptance",
                "units":            "m",
                "default":          3,
                "decimalPlaces":    2
            },
            "param3": {
                "label":            "PassThru",
                "units":            "m",
                "default":          0,
                "decimalPlaces":    2
            },
            "param4": {
                "label":            "Yaw",
                "units":            "deg",
                "nanUnchanged":     true,
                "default":          null,
                "decimalPlaces":    2
            }
        },
```

注意：在现实中，基于此的信息应由 mavlink 本身提供，而不需要成为 GCS 的一部分。

### 叶节点

然后，叶节点提供元数据，这些元数据可以覆盖命令的值和/或从显示给用户的参数中删除参数。 完整的树层次结构是这样的：

- 根－通用Mavlink
  - 特定的车辆类型－特定于车辆的通用规范
  - 特定的硬件类型－每个固件类型有一个可选的叶节点（PX4/ArduPilot）
     - 特定的车辆类型－每个车辆类型有一个可选的叶节点（FW/MR/VTOL/Rover/Sub）

注意：实际上，此替代功能应该是mavlink规格的一部分，并且应该可以从车辆中查询。

### 从完整树中构建实例树

由于 json 元数据提供了所有固件／车辆类型组合的信息，因此必须根据用于创建计划的固件和车辆类型来构建要使用的实际树。 这是通过一个进程调用“collapsing”的完整树到一个固件／车辆的特定树来完成的 ([code](https://github.com/mavlink/qgroundcontrol/blob/master/src/MissionManager/MissionCommandTree.cc#L119))。

步骤如下：
* 在实例树种添加根
* 将特定的车辆类型重写实例树
* 将特定的硬件类型重写实例树
* 将特定的硬件／车辆类型重写实例树

然后，生成的任务命令树将为平面项目编辑器构建UI。 实际上，它不仅用于此，还有许多其他地方可以帮助您了解有关特定命令id的更多信息。

## 层次结构示例 `MAV_CMD_NAV_WAYPOINT`

让我们来看看`MAV_CMD_NAV_WAYPOINT`的示例层次结构。 根信息如上图所示。

### Root - Vehicle Type Specific leaf node
The next level of the hiearchy is generic mavlink but vehicle specific. Json files are here: [MR](https://github.com/mavlink/qgroundcontrol/blob/master/src/MissionManager/MavCmdInfoMultiRotor.json), [FW](https://github.com/mavlink/qgroundcontrol/blob/master/src/MissionManager/MavCmdInfoFixedWing.json), [ROVER](https://github.com/mavlink/qgroundcontrol/blob/master/src/MissionManager/MavCmdInfoRover.json), [Sub](https://github.com/mavlink/qgroundcontrol/blob/master/src/MissionManager/MavCmdInfoSub.json), [VTOL](https://github.com/mavlink/qgroundcontrol/blob/master/src/MissionManager/MavCmdInfoVTOL.json). And here are the overrides for (Fixed Wings)(https://github.com/mavlink/qgroundcontrol/blob/master/src/MissionManager/MavCmdInfoFixedWing.json#L7):

```
        {
            "id":           16,
            "comment":      "MAV_CMD_NAV_WAYPOINT",
            "paramRemove":  "4"
        },
```

What this does is remove the editing UI for param4 which is Yaw and not used by fixed wings. Since this is a leaf node of the root, this applies to all fixed wing vehicle no matter the firmware type.

### Root - Firmware Type Specific leaf node
The next level of the hiearchy are overrides which are specific to a firmware type but apply to all vehicle types.  Once again lets loook at the waypoint overrides:

[ArduPilot](https://github.com/mavlink/qgroundcontrol/blob/master/src/FirmwarePlugin/APM/MavCmdInfoCommon.json#L6):

```
        {
            "id":           16,
            "comment":      "MAV_CMD_NAV_WAYPOINT",
            "paramRemove":  "2"
        },
```

[PX4](https://github.com/mavlink/qgroundcontrol/blob/master/src/FirmwarePlugin/PX4/MavCmdInfoCommon.json#L7):

```
        {
            "id":           16,
            "comment":      "MAV_CMD_NAV_WAYPOINT",
            "paramRemove":  "2,3"
        },
```

You can see that for both firmwares param2 which is acceptance radius is removed from the editing ui. This is a QGC specific decision. It is generally safer and easier to use the firmwares generic acceptance radius handling than the specify a value. So we've decided to hide it from users.

You can also see that for PX4 param3/PassThru is removed since it is not supported by PX.

### Root - Firmware Type Specific - Vehicle Type Specific leaf node
The last level of the hiearchy is both firmware and vehicle type specific.

[ArduPilot/MR](https://github.com/mavlink/qgroundcontrol/blob/master/src/FirmwarePlugin/APM/MavCmdInfoMultiRotor.json#L7):

```
        {
            "id":           16,
            "comment":      "MAV_CMD_NAV_WAYPOINT",
            "paramRemove":  "2,3,4"
        },
```

Here you can see that for an ArduPilot Multi-Rotor vehicle param2/3/4 Acceptance/PassThru/Yaw are removed. Yaw for example is removed because it is not supported. Due to quirk of how this code works, you need to repeat the overrides from the lower level.

## Mission Command UI Info
Two classes define the metadata associated with a command:

* MissionCommandUIInfo - Metadata for the entire command
* MissionCmdParamInfo - Metadata for a param in a command

The source is commented with full details of the json keys which are supported.

[MissionCommandUIInfo](https://github.com/mavlink/qgroundcontrol/blob/master/src/MissionManager/MissionCommandUIInfo.h#L82):

```
/// UI Information associated with a mission command (MAV_CMD)
///
/// MissionCommandUIInfo is used to automatically generate editing ui for a MAV_CMD. This object also supports the concept of only having a set of partial
/// information for the command. This is used to create overrides of the base command information. For on override just specify the keys you want to modify
/// from the base command ui info. To override param ui info you must specify the entire MissionParamInfo object.
///
/// The json format for a MissionCommandUIInfo object is:
///
/// Key                     Type    Default     Description
/// id                      int     reauired    MAV_CMD id
/// comment                 string              Used to add a comment
/// rawName                 string  required    MAV_CMD enum name, should only be set of base tree information
/// friendlyName            string  rawName     Short description of command
/// description             string              Long description of command
/// specifiesCoordinate     bool    false       true: Command specifies a lat/lon/alt coordinate
/// specifiesAltitudeOnly   bool    false       true: Command specifies an altitude only (no coordinate)
/// standaloneCoordinate    bool    false       true: Vehicle does not fly through coordinate associated with command (exampl: ROI)
/// isLandCommand           bool    false       true: Command specifies a land command (LAND, VTOL_LAND, ...)
/// friendlyEdit            bool    false       true: Command supports friendly editing dialog, false: Command supports 'Show all values" style editing only
/// category                string  Advanced    Category which this command belongs to
/// paramRemove             string              Used by an override to remove params, example: "1,3" will remove params 1 and 3 on the override
/// param[1-7]              object              MissionCommandParamInfo object
///
```

[MissionCmdParamInfo](https://github.com/mavlink/qgroundcontrol/blob/master/src/MissionManager/MissionCommandUIInfo.h#L25):

```
/// UI Information associated with a mission command (MAV_CMD) parameter
///
/// MissionCommandParamInfo is used to automatically generate editing ui for a parameter associated with a MAV_CMD.
///
/// The json format for a MissionCmdParamInfo object is:
///
/// Key             Type    Default     Description
/// label           string  required    Label for text field
/// units           string              Units for value, should use FactMetaData units strings in order to get automatic translation
/// default         double  0.0/NaN     Default value for param. If no default value specified and nanUnchanged == true, then defaultValue is NaN.
/// decimalPlaces   int     7           Number of decimal places to show for value
/// enumStrings     string              Strings to show in combo box for selection
/// enumValues      string              Values associated with each enum string
/// nanUnchanged    bool    false       True: value can be set to NaN to signal unchanged
```
