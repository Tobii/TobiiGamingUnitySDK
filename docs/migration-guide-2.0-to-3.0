# Migration Guide from version 2.0 to 3.0

## Contents

- [Summary](#summary)
- [Major Name Changes](#major-name-changes)
  - [Namespace: Tobii.Gaming](#namespace-tobiigaming)
  - [Static API: TobiiAPI](#static-api-tobiiapi)
- ["Advanced API" Made Internal](#advanced-api-made-internal)
- [Moved Functionality](#moved-functionality)
  - [Data Stream Initialization](#data-stream-initialization)
  - [Gaze Points Since](#gaze-points-since)
  - [Display Info](#display-info)
- [Removed Functionality](#removed-functionality)
  - [Unsubscribe Gaze Point Data](#unsubscribe-gaze-point-data)
  - [Screen Bounds](#screen-bounds)
  - [Gaze Tracking Status](#gaze-tracking-status)
  - [Eye Tracker Device Status](#eye-tracker-device-status)


## Summary

Though we had hoped that there would not be major changes from the 2.0 (Beta) version of the SDK to the next version, some unforseen circumstances (aren't there always?) and a move to another underlying eye tracking engine put us in a position where we had to break hard with the previous version of the API no matter what we did. We therefore decided to make an overall review of the API and framework, and have made the total public API more convenient to use, with all relevant functionality directly available as functions on the static `TobiiAPI` (previously `EyeTracking`) class. Some functionality has been removed indefinately, some other functionality is missing but might be re-added later if/when made available from the underlying eye tracking engine.


## Major Name Changes

### Namespace: Tobii.Gaming

```csharp
// v2.0
using Tobii.EyeTracking;

// v3.0
using Tobii.Gaming;

```

### Static API: TobiiAPI

```csharp
// v2.0
GazePoint gazePoint = EyeTracking.GetGazePoint();

// v3.0
GazePoint gazePoint = TobiiAPI.GetGazePoint();

```

## "Advanced API" Made Internal

We have moved all relevant functions from the "Advanced API" to the static `TobiiAPI`, making access to the `ITobiiHost` reduntant. We have therefore made the `ITobiiHost` and associated classes internal, put them in an internal namespace (`Tobii.Gaming.Internal`) and folder called `Internal`. These classes are no longer part of the public API. See next sections for more information.

## Moved Functionality

Some functions have been split up to separate more specific functions. Some functionality that were only in the "Advanced API" have now been added to the static TobiiAPI.

### Data Stream Initialization

```csharp
// v2.0
EyeTracking.Initialize();

// v3.0
TobiiAPI.SubscribeGazePointData();
TobiiAPI.SubscribeHeadPoseData();
```

### Gaze Points Since

```csharp
// v2.0
// "Advanced API"
GazePoint lastHandledGazePoint;
IDataProvider<GazePoint> gazePointDataProvider = EyeTrackingHost.GetGazePointDataProvider();
// ...
IEnumerable<GazePoint> newGazePoints = gazePointDataProvider.GetDataPointsSince(lastHandledGazePoint);

// v3.0
// Static TobiiAPI
GazePoint lastHandledGazePoint;
// ...
IEnumerable<GazePoint> newGazePoints = TobiiAPI.GetGazePointsSince(lastHandledGazePoint);
```

### Display Info

```csharp
// v2.0
// "Advanced API"
IStateValue<Vector2> displaySize = EyeTrackingHost.GetInstance().DisplaySize;
if (displaySize.IsValid)
{
    print("displaySize (width,height): " + displaySize.x + ", " + displaySize.y);
}

// v3.0
// Static TobiiAPI
DisplayInfo displayInfo = TobiiAPI.GetDisplayInfo();
if (displayInfo.IsValid)
{
    print("displayInfo (width,height): " + displayInfo.DisplayWidthMm + ", " + displayInfo.DisplayHeightMm);
}
```


## Removed Functionality

The 3.0 SDK is built on top of a different more low-level eye tracking engine than the 2.0 SDK. This means that some functionality that was available in 2.0 cannot be provided (at least yet) in the 3.0 version. Below are some examples of removed functions and functionality and what we suggest to use/do instead.


### Unsubscribe Gaze Point Data

```csharp
// v2.0
UnsubscribeGazePointData();

// v3.0
// (nothing)

```

### Screen Bounds

```csharp
// v2.0
IStateValue<UnityEngine.Rect> screenBounds = EyeTrackingHost.GetInstance().ScreenBounds;

// v3.0
// (nothing - we cannot get this information from the eye tracking engine we are now using, 
//  it might/should be added in a future version of the SDK)
```

### Gaze Tracking Status

The underlying eye tracking engine we are now using does not provide gaze tracking information. In order to know if the user's eye-gaze is currently tracked, one has to instead check either the recentness of the latest gaze point or the user presence status. For convenience we have added a couple of utility functions to check if a gaze point is recent.

```csharp
// v2.0
EyeTracking.GetGazeTrackingStatus();

// v3.0
GazePoint gazePoint = TobiiAPI.GetGazePoint();
if (gazePoint.IsRecent()) // default max age 500ms
{
    // ...
}

float MaxAge = 0.2f // to check if newer than 200ms
if (gazePoint.IsRecent(MaxAge))
{
    // ...
}


if (TobiiAPI.GetUserPresence().IsUserPresent)
{
    // ...
}
```


### Eye Tracker Device Status

It is no longer possible for us to read the status of the eye tracker if it is connected, setup and so forth. We suggest designing your code to always only act if there is recent (meaning also valid) data or a user present in front of the eye tracker.

```csharp
// v2.0
EyeTracking.GetGazeTrackingStatus();

// v3.0
GazePoint gazePoint = TobiiAPI.GetGazePoint();
if (gazePoint.IsRecent())
{
    // ...
}

if (TobiiAPI.GetUserPresence().IsUserPresent)
{
    // ...
}
```
