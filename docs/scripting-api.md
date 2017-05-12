---
title: "Scripting API"
---
# Scripting API

>*SDK version: 3.0 Beta*

## Contents

- [TobiiAPI](#tobiiapi)
  - [GetDisplayInfo](#tobiiapigetdisplayinfo)
  - [GetFocusedObject](#tobiiapigetfocusedobject)
  - [GetGazePoint](#tobiiapigetgazepoint)
  - [GetGazePointsSince](#tobiiapigetgazepointssince)
  - [GetHeadPose](#tobiiapigetheadpose)
  - [GetHeadPosesSince](#tobiiapigetheadposessince)
  - [GetUserPresence](#tobiiapigetuserpresence)
  - [SetCurrentUserViewPointCamera](#tobiiapisetcurrentuserviewpointcamera)
  - [SubscribeGazePointData](#tobiiapisubscribegazepointdata)
  - [SubscribeHeadPoseData](#tobiiapisubscribeheadposedata)
- [Components](#components)
  - [GazeAware](#gazeaware)
- [Data Types and Enums](#data-types-and-enums)
  - [DisplayInfo](#displayinfo)
  - [GazePoint](#gazepoint)
  - [HeadPose](#headpose)
  - [UserPresence](#userpresence)

____________________________________________________________________________________________________

## TobiiAPI

static class in `Tobii.Gaming` namespace

The TobiiAPI is a static API that provides direct access to the most common and useful functionality of the Tobii Unity SDK framework.

[&rarr; View in Manual](manual#tobii-api)


### Description

`TobiiAPI` is a static class with static methods to easily access data from a connected Tobii eye tracker.

To get access to `TobiiAPI`, import the Tobii Unity SDK into your project and add the following using (at the top of the script file where you want to use the Tobii API):

```csharp
using Tobii.Gaming;
```

`TobiiAPI` provides simplified and straightforward access to eye tracker data and can be used for the most common use cases.

`TobiiAPI` also provides information about which `UnityEngine.GameObject` the user focuses using eye-gaze. Only gaze aware objects will be considered for gaze focus detection. The way to make an object gaze-aware is to add the [`GazeAware`](#gazeaware) component to it.

### Static Functions

| Name  | Description |
| :---- | :---------- |
| [GetDisplayInfo](#tobiiapigetdisplayinfo) | Gets info about the eye tracked display monitor. |
| [GetFocusedObject](#tobiiapigetfocusedobject) | Gets the game object with gaze focus. |
| [GetGazePoint](#tobiiapigetgazepoint) | Gets the last (newest) [`GazePoint`](#gazepoint). |
| [GetGazePointsSince](#tobiiapigetgazepointssince) | Gets the gaze points since a given [`GazePoint`](#gazepoint). |
| [GetHeadPose](#tobiiapigetheadpose) | Gets the last (newest) [`HeadPose`](#headpose) |
| [GetHeadPosesSince](#tobiiapigetheadposessince) | Gets the head poses since a given [`HeadPose`](#headpose). |
| [GetUserPresence](#tobiiapigetuserpresence) | Gets the user presence status. |
| [SetCurrentUserViewPointCamera](#tobiiapisetcurrentuserviewpointcamera) | Sets the camera used for gaze focus detection. |
| [SubscribeGazePointData](#tobiiapisubscribegazepointdata)   | Subscribes to gaze point data. |
| [SubscribeHeadPostData](#tobiiapisubscribeheadposedata)     | Subscribes to head pose data. |

[&uarr; Back to Section start](#tobiiapi) | [&uarr; Back to Top](#scripting-api)


## TobiiAPI.GetDisplayInfo

static function on [`TobiiAPI`](#tobiiapi)

`public static` [`DisplayInfo`](#displayinfo) `GetDisplayInfo()`

### Returns

| Type | Description |
| :--- | :---------- |
| [`DisplayInfo`](#displayinfo) | Value object with information about the eye tracked display monitor. Can be invalid, check `IsValid` before using. |

### Description

Gets a [`DisplayInfo`](#displayinfo) value object with information about the eye tracked display monitor.

[&uarr; Back to Section start](#tobiiapi) | [&uarr; Back to Top](#scripting-api)


## TobiiAPI.GetFocusedObject

static function on [`TobiiAPI`](#tobiiapi)

`public static UnityEngine.GameObject GetFocusedObject()`
[&rarr; View related entry in Manual](manual#gaze-focus-and-gazeaware-component)


### Returns

| Type | Description |
| :--- | :---------- |
| `UnityEngine.GameObject` | The object the user is focused on by looking at it. Can be `null` if no object is focused. |

### Description

Gets the game object with gaze focus, or returns null if there is no game object with gaze focus.

Only game objects that are gaze-aware will be considered for gaze focus detection. The easiest way to make a game object gaze-aware is to add the [`GazeAware`](#gazeaware) component to it. Another way is to implement your own custom gaze-aware component, see [`IGazeFocusable`](#igazefocusable).

```csharp
using Unity.Engine;
using Tobii.Gaming;

public class ExampleClass : MonoBehaviour
{
    void Update()
    {
        GameObject focusedObject = TobiiAPI.GetFocusedObject();
        if (null != focusedObject)
        {
            print("The focused game object is: " + focusedObject.name + " (ID: " + focusedObject.GetInstanceID() + ")");
        }
    }
}
```

[&uarr; Back to Section start](#tobiiapi) | [&uarr; Back to Top](#scripting-api)


## TobiiAPI.GetGazePoint

static function on [`TobiiAPI`](#tobiiapi)

`public static` [`GazePoint`](#gazepoint) `GetGazePoint()`

[&rarr; View related entry in Manual](manual#gaze-point-data)

### Returns

| Type | Description |
| :--- | :---------- |
| [`GazePoint`](#gazepoint) | Where on the screen the user is looking. Can be invalid, check `GazePoint.IsValid` before using or use `GazePoint.IsRecent()` function to ensure that it's both valid and recent. |


### Description

Gets the last [`GazePoint`](#gazepoint).

The [`TobiiAPI`](#tobiiapi) class is initialized lazily if [`TobiiAPI.SubscribeGazePointData()`](#tobiiapisubscribegazepointdata) is not called first. This means that the data provider is started at the first call to this function (or the [`TobiiAPI.GetFocusedObject()`](#tobiiapigetfocusedobject) function), and the function will return an invalid value for some frames until valid data is received from the eye tracker.

```csharp
using Unity.Engine;
using Tobii.Gaming;

public class ExampleClass : MonoBehaviour
{
    void Update()
    {
        GazePoint gazePoint = TobiiAPI.GetGazePoint();
        if (gazePoint.IsRecent()) // Use IsValid property instead to process old but valid data
        {
            // Note: Values can be negative if the user looks outside the game view.
            print("Gaze point on Screen (X,Y): " + gazePoint.Screen.X + ", " + gazePoint.Screen.Y);
        }
    }
}
```

[&uarr; Back to Section start](#tobiiapi) | [&uarr; Back to Top](#scripting-api)


## TobiiAPI.GetGazePointsSince

static function on [`TobiiAPI`](#tobiiapi)

`public static IEnumerable<GazePoint> GetGazePointsSince(`[`GazePoint`](#gazepoint) `gazePoint)`

### Parameters

| Name      | Type           | Description |
| :-------- | :------------- | :---------- |
| gazePoint | [`GazePoint`](#gazepoint) | Typically an already handled gaze point. The function will return all gaze points newer than this gaze point. |

### Returns

| Type | Description |
| :--- | :---------- |
| `IEnumerable<GazePoint>`  | All the [`GazePoint`](#gazepoint)s that have been received from the eye tracker since the supplied gaze point.|

### Description

Gets all [`GazePoint`](#gazepoint)s that have been received from the eye tracker since the supplied gaze point, but no older than 500 ms.

This function is useful when you want to do your own advanced filtering of the gaze point data and include all received gaze points in the calculation. If you use [`GetGazePoint()`](#tobiiapigetgazepoint) you will likely miss some points from the eye tracker, since the game and the eye tracker will probably not run in the exact same frequency.

You can save your last handled data point in the Update loop, and use it as a parameter for this function in the next Update loop to retrieve all new data points.


```csharp
using System.Collections.Generic;
...

public class ExampleClass : MonoBehaviour
{
    private GazePoint _lastHandledPoint;

    Update()
    {
        IEnumerable<GazePoint> pointsSinceLastHandled = TobiiAPI.GetGazePointsSince(_lastHandledPoint);
        foreach (point in pointsSinceLastHandled)
        {
            // handle each point that has arrived since previous Update()
            // ...
        }
        _lastHandledPoint = pointsSinceLastHandled.Last();
    }
}
```

[&uarr; Back to Section start](#tobiiapi) | [&uarr; Back to Top](#scripting-api)


## TobiiAPI.GetHeadPose

static function on [`TobiiAPI`](#tobiiapi)

`public static` [`HeadPose`](#headpose) `GetHeadPose()`

[&rarr; View related entry in Manual](manual#head-pose-data)

### Returns

| Type | Description |
| :--- | :---------- |
| [`HeadPose`](#headpose) | Where on the screen the user is looking. Can be invalid, check `HeadPose.IsValid` before using or `HeadPose.IsRecent()` to check that it's both valid and recent. |

### Description

Gets the last [`HeadPose`](#headpose).

The [`TobiiAPI`](#tobiiapi) class is initialized lazily if [`TobiiAPI.SubscribeHeadPoseData()`](#tobiiapisubscribeheadposedata) is not called first. This means that the data provider is started at the first call to this function, and the function will return an invalid value for some frames until valid data is received from the eye tracker.

```csharp
using Unity.Engine;
using Tobii.Gaming;

public class ExampleClass : MonoBehaviour
{
    void Update()
    {
        HeadPose headPose = TobiiAPI.GetHeadPose();
        if (headPose.IsRecent())
        {
            print("HeadPose Position (X,Y,Z): " + headPose.Position.X + ", " + headPose.Position.Y + ", " + headPose.Position.Z);
            print("HeadPose Rotation (X,Y,Z): " + headPose.Rotation.eulerAngles.X + ", " + headPose.Rotation.eulerAngles.Y + ", " + headPose.Rotation.eulerAngles.Z);
        }
    }
}
```

[&uarr; Back to Section start](#tobiiapi) | [&uarr; Back to Top](#scripting-api)


## TobiiAPI.GetHeadPosesSince

static function on [`TobiiAPI`](#tobiiapi)

`public static IEnumerable<HeadPose> GetHeadPosesSince(`[`HeadPose`](#headpose) `headPose)`

### Parameters

| Name      | Type           | Description |
| :-------- | :------------- | :---------- |
| headPose | [`HeadPose`](#headpose) | Typically an already handled head pose. The function will return all head poses newer than this head pose. |

### Returns

| Type | Description |
| :--- | :---------- |
| `IEnumerable<HeadPose>`  | All the [`HeadPose`](#headpose)s that have been received from the eye tracker since the supplied head pose.|

### Description

Gets all [`HeadPose`](#headpose)s that have been received from the eye tracker since the supplied head pose, but no older than 500 ms.

This function is useful when you want to do your own advanced filtering of the head pose data and include all received head poses in the calculation. If you use [`GetHeadPose()`](#tobiiapigetheadpose) you will likely miss some points from the eye tracker, since the game and the eye tracker will probably not run in the exact same frequency.

You can save your last handled data point in the Update loop, and use it as a parameter for this function in the next Update loop to retrieve all new data points.


```csharp
using System.Collections.Generic;
...

public class ExampleClass : MonoBehaviour
{
    private HeadPose _lastHandledPoint;

    Update()
    {
        IEnumerable<HeadPose> pointsSinceLastHandled = TobiiAPI.GetHeadPosesSince(_lastHandledPoint);
        foreach (point in pointsSinceLastHandled)
        {
            // handle each point that has arrived since previous Update()
            // ...
        }
        _lastHandledPoint = pointsSinceLastHandled.Last();
    }
}
```

[&uarr; Back to Section start](#tobiiapi) | [&uarr; Back to Top](#scripting-api)


## TobiiAPI.GetUserPresence

static function on [`TobiiAPI`](#tobiiapi)

`public static` [`UserPresence`](#userpresence) `GetUserPresence()`

### Returns

| Type | Description |
| :--- | :---------- |
| [`UserPresence`](#userpresence) | User presence status, indicating if there is a user in front of the eye tracker. |

### Description

Get the user presence, which indicates if there is a user present in front of the screen/eye tracker.

The [`UserPresence`](#userpresence) extension method `IsUserPresent()` returns true if the system detects a user in front of the eye tracker. It is false if the system cannot detect a user in front of the eye tracker, but it is also false if the user presence status is [`UserPresence.Unknown`](#userpresence). This means that this property can give false negatives if the user presence status is unknown. For example during initializing, or if eye tracking has been disabled by the user, the user can be in front of the eye tracker but `IsUserPresent()` returns false. If you need to avoid false negatives for these special cases, check enum value instead.

```csharp
using Unity.Engine;
using Tobii.Gaming;

public class ExampleClass : MonoBehaviour
{
    void Update()
    {
        UserPresence userPresence = TobiiAPI.GetUserPresence();
        if (userPresence.IsUserPresent())
        {
            print("A user is present in front of the screen.");
        }

        print("User presence status is: " + userPresence);
    }
}
```

[&uarr; Back to Section start](#tobiiapi) | [&uarr; Back to Top](#scripting-api)


## TobiiAPI.SetCurrentUserViewPointCamera

static function on [`TobiiAPI`](#tobiiapi)

`public static void SetCurrentUserViewPointCamera(Camera camera)`

### Parameters

| Name   | Type                 | Description |
| :----- | :------------------- | :---------- |
| camera | `UnityEngine.Camera` | The camera that defines the user's current view point. |

### Description

Sets the camera that defines the user's current view point. The camera is used for gaze focus detection.

By default `Camera.main` is used for gaze focus detection to decide which game object the user's eye-gaze is focused on. If the main camera is not the camera that defines the user's current view point, this function can be used to override the main camera with a custom camera.

To decide which camera corresponds to the user's view point, it should fulfill the following:

- given the current `GazePoint gazePoint`
- when calling `camera.ScreenPointToRay(gazePoint.Screen)`
- the resulting ray into world space should be a continuation of the "ray" from the user's eyes to the gaze point on the  display monitor

[&uarr; Back to Section start](#tobiiapi) | [&uarr; Back to Top](#scripting-api)


## TobiiAPI.SubscribeGazePointData

static function on [`TobiiAPI`](#tobiiapi)

`public static void SubscribeGazePointData()`

### Description

Starts the gaze point data provider.

Use this method if you want to initialize the gaze point data provider explicitly rather than to use the default lazy initialization. If this method is not used, the gaze point data provider will be initialized and started at the first call to [`TobiiAPI.GetGazePoint()`](#tobiiapigetgazepoint) or [`TobiiAPI.GetFocusedObject()`](#tobiiapigetfocusedobject), and there will be some frames before that method returns valid gaze points.

[&uarr; Back to Section start](#tobiiapi) | [&uarr; Back to Top](#scripting-api)


## TobiiAPI.SubscribeHeadPoseData

static function on [`TobiiAPI`](#tobiiapi)

`public static void SubscribeHeadPoseData()`

### Description

Starts the head pose data provider.

Use this method if you want to initialize the head pose data provider explicitly rather than to use the default lazy initialization. If this method is not used, the head pose data provider will be initialized and started at the first call to [`TobiiAPI.GetHeadPose()`](#tobiiapigetheadpose), and there will be some frames before that method returns valid gaze points.

[&uarr; Back to Section start](#tobiiapi) | [&uarr; Back to Top](#scripting-api)

____________________________________________________________________________________________________

## COMPONENTS

This section describes components that can be used out-of-the-box to add eye-gaze features to game objects.

[&uarr; Back to Top](#scripting-api)

## GazeAware

class in `Tobii.Gaming` namespace / Inherits from: `UnityEngine.MonoBehaviour` / Implements: [`IGazeFocusable`](#igazefocusable)
[&rarr; View related entry in Manual](manual#gaze-focus-and-gazeaware-component)

### Description

Makes the game object it is attached to gaze-aware, meaning aware of the user's eye-gaze on it.

The `HasGazeFocus` property indicates if the user is currently looking at the game object or not. Which gaze-aware game object that currently has gaze focus can also be queried by calling [`TobiiAPI.GetFocusedObject()`](#tobiiapigetfocusedobject). Only game objects with the `GazeAware` component or some other component that implements the [`IGazeFocusable`](#igazefocusable) interface can get gaze focus.

### Properties

| Name         | Type   | Description |
| :----------- | :----- | :---------- |
| HasGazeFocus | `bool` | True if the game object it is attached to has gaze focus, false otherwise. |

[&uarr; Back to Section start](#components) | [&uarr; Back to Top](#scripting-api)


____________________________________________________________________________________________________

## DATA TYPES AND ENUMS

This section describes the data types and enums used with the [`TobiiAPI`](#tobiiapi).

- [GazePoint](#gazepoint)
- [HeadPose](#headpose)
- [UserPresence](#userpresence)

[&uarr; Back to Top](#scripting-api)


## DisplayInfo

value type in `Tobii.Gaming` namespace

### Description

The `DisplayInfo` struct contains information about the eye-tracked display monitor.

### Properties

| Name                 | Type      | Description |
| :------------------- | :-------- | :---------- |
| DisplayWidthMm       | `float` | The width in millimeters of the eye-tracked display monitor. |
| DisplayHeightMm      | `float` | The height in millimeters of the eye-tracked display monitor. |
| IsValid              | `float` | True if the info object is valid, false otherwise. |

### Static properties

| Name    | Description |
| :------ | :---------- |
| Invalid | Creates a value representing an invalid `DisplayInfo` instance. |

[&uarr; Back to Section start](#data-types-and-enums) | [&uarr; Back to Top](#scripting-api)


## GazePoint

value type in `Tobii.Gaming` namespace

[&rarr; View related entry in Manual](manual#gaze-point-data)

### Description

A `GazePoint` represents a point on the screen where the user is looking (or where the user's eye-gaze intersects with the screen plane).

Always check if a `GazePoint` is valid before using it, checking `IsValid` property or use `IsRecent()` function to ensure that it's both valid and recent. Invalid points will be returned by the Tobii Unity SDK framework during startup and shutdown, a few frames after calling [`TobiiAPI.GetGazePoint()`](#tobiiapigetgazepoint) (or [`TobiiAPI.SubsribeGazePointData()`](#tobiiapisubscribegazeposedata)) for the first time, and if the data provider is stopped for some reason. Invalid points will always be returned on unsupported standalone platforms, currently Mac and Linux.

The `PreciseTimestamp` can be used to compare the timestamps between two `GazePoint`s with high accuracy.

### Properties

| Name                 | Type      | Description |
| :------------------- | :-------- | :---------- |
| GUI                  | `Vector2` | Gaze point in GUI space (where (0,0) is upper left corner). |
| IsValid              | `bool`    | True if the point is valid, false otherwise. |
| PreciseTimestamp     | `long`    | Precise timestamp of the gaze point. |
| Screen               | `Vector2` | Gaze point in screen space (where (0,0) is lower left corner). |
| Timestamp            | `float`   | `Time.unscaledTime` timestamp of the gaze point. |
| Viewport             | `Vector2` | Gaze point in viewport space. |

### Static properties

| Name | Description |
| :--- | :--- |
| Invalid       | Creates a value representing an invalid `GazePoint` |

[&uarr; Back to Section start](#data-types-and-enums) | [&uarr; Back to Top](#scripting-api)

### Functions

| Name  | Description |
| :---- | :---------- |
| IsRecent | Returns true if the point is valid and recent, false otherwise. |
| IsRecent(float maxAge) | Returns true if the point is valid and no older than `maxAge` seconds, false otherwise. |


## HeadPose

value type in `Tobii.Gaming` namespace

[&rarr; View related entry in Manual](manual#head-pose-data)

### Description

A `HeadPose` represents the head position and rotation of the user's head.

Always check if a `HeadPose` is valid before using it. Invalid points will be returned by the Tobii Unity SDK framework during startup and shutdown, a few frames after calling [`TobiiAPI.GetHeadPose()`](#tobiiapigetheadpose) (or [`TobiiAPI.SubscribeHeadPoseData()`](#tobiiapisubscribeheadposedata)) for the first time, and if the data provider is stopped for some reason. Invalid points will always be returned on unsupported standalone platforms, currently Mac and Linux.

The `PreciseTimestamp` can be used to compare the timestamps between two `HeadPose`s with high accuracy.

### Properties

| Name      | Type          | Description |
| :-------- | :------------ | :---------- |
| IsValid   | `bool`        | True if the head pose is valid, false otherwise. |
| Position  | `Vector3`     | (X, Y, Z) position of the head of the user, in millimeters from the center of the eye tracked display monitor's screen area. |
| PreciseTimestamp | `long` | The precise timestamp of the head pose. |
| Rotation  | `Quaternion`  | Rotation of the head of the user, expressed using `Quternion`. Use `eulerAngles` property of the `Quaternion` to convert it to euler angles. |
| Timestamp | `float`       | The `Time.unscaledTime` timestamp of the gaze point. |

### Static properties

| Name    | Description |
| :------ | :---------- |
| Invalid | Creates a value representing an invalid `HeadPose` |

### Functions

| Name  | Description |
| :---- | :---------- |
| IsRecent | Returns true if the head pose is valid and recent, false otherwise. |
| IsRecent(float maxAge) | Returns true if the head pose is valid and no older than `maxAge` seconds, false otherwise. |

[&uarr; Back to Section start](#data-types-and-enums) | [&uarr; Back to Top](#scripting-api)


## UserPresence

enum in `Tobii.Gaming` namespace

### Description

Describes possible user presence statuses. See also [`TobiiAPI.GetUserPresence()`](#tobiiapigetuserpresence).

### Values

| Name       | Description |
| :--------- | :---------- |
| Unknown    | User presence is unknown, the system might for example be initializing |
| Present    | User is present |
| NotPresent | User is not present |

[&uarr; Back to Section start](#data-types-and-enums) | [&uarr; Back to Top](#scripting-api)

### Extension Method

#### IsUserPresent

extension method for `UserPresence`

`public static bool IsUserPresent(this UserPresence userPresence)`

Returns true if `UserPresence` is [`UserPresence.Present`](#userpresence), false otherwise.

`IsUserPresent()` can give false negatives for example during initialization when the user presence status is [`UserPresence.Unknown`](#userpresence).