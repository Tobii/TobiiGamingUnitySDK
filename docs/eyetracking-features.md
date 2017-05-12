---
title: "Eye Tracking Features"
---
# Tobii Eye Tracking Features

## Contents

- [Features Overview](#features-overview)
- [Extended View](#extended-view)
- [Clean UI](#clean-ui)
- [Aim at Gaze](#aim-at-gaze)
- [Interact at Gaze](#interact-at-gaze)
- [Gaze Awareness](#gaze-awareness)
- [Map Navigation](#map-navigation)
- [Dynamic Light Adaptation](#dynamic-light-adaptation)

## Features Overview

An eye tracker knows where you are looking in real-time, enabling a new way of interacting with your game. It does not replace - but rather complements the keyboard, mouse and gamepad as an input method when playing games.

The eye tracker provides [Gaze Point](manual#gaze-point-data) data, points on the screen where you are looking. You can use this data to make your game react or adapt to where the player's eye-gaze is pointed. Some Tobii eye trackers also provide [Head Pose](manual#head-pose-data) data, that can be used to complement Gaze Point data as input, for example when rotating the camera based on a combination of the two types of data.

Here are some examples how it works to make your game more immersive:

- Look around freely [&raquo; Extended View](#extended-view)
- Get information on-demand [&raquo; Clean UI](#clean-ui)
- Aim where you look [&raquo; Aim at Gaze](#aim-at-gaze)
- Interact where you look [&raquo; Interact at Gaze](#interact-at-gaze)
- The game reacts to your gaze [&raquo; Gaze Awareness](#gaze-awareness)
- Navigate where you look [&raquo; Map Navigation](#map-navigation)
- Light adaptation to different lighting scenarios [&raquo; Dynamic Light Adaptation](#dynamic-light-adaptation)

We call these different Eye Tracking based experiences our Tobii Eye Tracking Features. The above is just a small subset of what we are exploring at Tobii and in the Tobii Gaming Team.

-------------------------------------------------------------------------------

## Extended view

>"Look around freely"

Extended View directs the in-game camera to rotate or orbit depending on your head rotation and gaze point position on the screen, automatically moving what you want to look at into view, creating what feels like an automatic panning effect of the field of view. It lets you look around freely (within some specified angles appropriate for the game) instead of having the camera view completely locked to the forward move direction of the game character or vehicle. This means that Extended View rotates the camera without changing the character's forward move direction. The crosshair is still controlled by traditional input methods like mouse and gamepad. Extended View will not center the game view on the gaze point, but rather move it closer to the center.

Here is a video that shows how Extended View works in Rise of The Tomb Raider:

![](http://i.imgur.com/B1uZygu.gif)

[&uarr; Back to Top](#tobii-eye-tracking-features)

-------------------------------------------------------------------------------

## Clean UI

>"Get information on-demand"

Using eye tracking, HUD Elements such as maps, health bars and ammo bars can be faded and transparent when not looked at, keeping the screen clean from unnecessary items. When looked at, these elements are displayed opaque, providing the info only when you need it.

Without eye tracking - all UI elements always fully visible. With eye tracking and Clean UI (the white circle to the right illustrates where the user is looking) - only the UI elements the user looks at are visible.

![](http://i.imgur.com/qGe2iMU.gif)

### Algorithm - Clean UI

The algorithm uses a screen area for each Clean UI element, that covers and is slightly bigger than the UI element, to check if the latest gaze point falls inside or outside of this area.

If the gaze point is inside the area, the opacity of the UI element is lerped fast towards full opaqueness. Fast in this case is a value of around a couple of hundred milliseconds.

If the gaze point falls outside the area, the transparency of the UI element is lerped slowly towards (almost) full transparency. Slow in this case is a value of around one second.

For some UI elements it might be suitable to add a delay before starting to lerp towards full transparency so that the element stays fully opaque a little while after the user has stopped looking at it. (The Clean UI scripts included in the Tobii Gaming SDK for Unity do not have this kind of delay.)

Another way to extend this algorithm could be to always display new messages at full transparency until they are looked at.

[&uarr; Back to Top](#tobii-eye-tracking-features) 

-------------------------------------------------------------------------------

## Aim at Gaze

>"Aim where you look"

Aim at Gaze is an eye tracking feature that comes in different flavors. It always include looking at a target somewhere on a screen and pressing a button to initiate aiming. What happens next is different.

In some games the next step is to change the ADS view, centered and zoomed in around the point the user looked at, then letting the user fine tune the aim on the target and fire at will.

In other games it makes more sense to use some kind of aim assist, where the aim locks on the target closest to the gaze point when the button is pressed. In this case there is no need to zoom in, just look at the target, press the aim button and fire.

In yet other games the Aim at Gaze might need to be tweaked in some other way. The most important thing is to find an interaction that fits with the game genre, style and game play.

![](images/TR_AimAtGaze.gif)

[&uarr; Back to Top](#tobii-eye-tracking-features) 

-------------------------------------------------------------------------------

## Interact at Gaze

>"Interact where you look"

Interact at gaze allows the player to interact with an object in a game by pressing a key (or giving some other kind of active or intentional input) while looking at it. The interaction could be anything from opening a door, loot a body to select another character for some sort of interaction. In many of the games that have integrated Tobii EyeTracking, these kinds of features have been given game specific names. For example 'Target at gaze' in Watch_Dogs 2 where you use your eyes to select targets to hack and characters to profile.

![](http://i.imgur.com/iletKkV.gif)


### Algorithm - Interact at Gaze

The Tobii Gaming SDK for Unity comes with built-in support for basic gaze-to-object-mapping. It uses ray casts to find which object is hit by each gaze point and keeps a score to decide which object is most likely looked at at any given moment. There are time thresholds so that only recent enough hits are considered, but also an onset threshold to avoid a too flickery behavior when a series of gaze points hits two objects close to each other.

For more information on how to use the Unity implementation, see the [Gaze Focus and GazeAware component](manual#gaze-focus-and-gazeaware-component) section in the User Manual.


[&uarr; Back to Top](#tobii-eye-tracking-features) 

-------------------------------------------------------------------------------

## Gaze Awareness

>"The game reacts to your gaze"

Gaze Awareness is a whole category of eye-gaze based interactions. It includes:

- Response to Eye Contact - have NPC's react when made eye contact with
- Passive Gaze Triggers - use the user's gaze point as a clue to what his or her intention is
- Peripheral Effects - have things appear or lurk in the peripheral vision, but disappear when looked straight at
- Affect Environment - have the environment change or react to being looked at, or have a scary monster appear right where you are looking in a dark corner of a horror game

![](http://i.imgur.com/uIuTeog.gif)

### Algorithm - Gaze Awareness

To implement gaze awareness the gaze point can be used to make things appear or happen where the user is looking. Depending on the feature, one might want to filter the gaze point data to know if the user is focusing their gaze on a certain point, or if the gaze is moving over the screen.

For gaze awareness features tied to looking at a specific object, the gaze-to-object-mapping approach, described in the [Interact at gaze](#interact-at-gaze) section, can be used.


[&uarr; Back to Top](#tobii-eye-tracking-features) 

-------------------------------------------------------------------------------

## Map Navigation

>"Navigate where you look"

Map Navigation is used for 2D navigation (specifically useful in strategy games) using the gaze point to decide where to move. It usually integrates two different navigation techniques into one algorithm:

- Center at gaze - a short press on a map navigation key centers the map where you are looking
- Bungee Zoom - a long press on the map navigation key zooms out the map for overview, when the key is released, the map is zoomed in centered on the gaze point

![](http://i.imgur.com/9kun5q5.gif)

### Algorithm - Map Navigation

    1. When Map Navigation key is pressed
       - Begin counting time
       - Record current zoom level as Initial Zoom Level
    2. If key is released before 0.3 seconds has passed:
       - Perform Center at gaze
       Else:
       - Zoom out to the max zoom-out level
    4. When key is released:
       - If the key was released before fully zoomed out:
         - cancel Bungee Zoom and go back to the Initial Zoom Level
       - Else If Initial Zoom Level was almost or fully zoomed out:
         - while zooming in to a medium zoom level, pan to center
         on the gaze point
       - Else (normal case):
         - while zooming back to the Initial Zoom Level, pan to 
         center on the gaze point 


[&uarr; Back to Top](#tobii-eye-tracking-features) 

-------------------------------------------------------------------------------

## Dynamic Light Adaptation

When the human eye moves between areas of different amounts of light exposure, it adapts to the new environment over time. You have probably experienced this when going indoors after being outside for an extended amount of time during a sunny summer day. The interior will look darker than normal for about ten minutes before going back to normal. This eye behavior is simulated in many modern games, but to make the effect more dramatic, the time window is often changed an order of magnitude or two, letting you see the light change over 10-30 seconds instead of over the real ten to twenty minutes.

![](http://i.imgur.com/btO1uGP.gif)

### Algorithm - Dynamic Light Adaptation

Without eye tracking, most games implement light adaptation by measuring the average light intensity of the current video frame and use this value as input to a tonemapping function that brightens or darkens parts of the screen.

Using the gaze position the algorithm can be improved to sample the light close to where the user is looking more than the other light intensity levels. This essentially lets the user peer into darker areas of the screen, and lets the game gradually adapt to that darker light level, and then return to normal when the user looks back into the well lit parts.

This can also be taken even further, by letting light sources blind the player, or introduce burn-in effects which physically is when light receptors in the eye are over or under-stimulated compared to the other receptors that leave "ghost-images" on the player's retina for a short while.

Here is a good article on the eyes' light and dark adaptation: [http://webvision.med.utah.edu/book/part-viii-gabac-receptors/light-and-dark-adaptation/](http://webvision.med.utah.edu/book/part-viii-gabac-receptors/light-and-dark-adaptation/)
That article also contains some nice information on after-imaging and over/under-stimulation.


[&uarr; Back to Top](#tobii-eye-tracking-features) 