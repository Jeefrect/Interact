# Interact Animation Guide

This guide describes how to make Kino custom objects compatible with Interact. The Interact mod triggers animations based on a specific naming format that it recognizes.

The mod works with KINO custom objects and KINO car parts only! 

Recommended Animation Setup in Unity for Interact Mod: [Animation-Recommendations.md](https://github.com/Jeefrect/Interact/blob/main/docs/Animation-Recommendations.md)

## Clip naming

Use this naming format for new animations:

```text
[<Action>[_<Group>]]<ID>
```

Supported actions:

```text
Open
Close
Loop
SpeedActive
```

![Docs Interact Mod](https://github.com/Jeefrect/Interact/blob/main/docs/doc-3.png)

Examples of standalone interactions:

```text
[Open]Hood
[Close]Hood

[Open]Rear_Left_Door
[Close]Rear_Left_Door

[Open]Trunk

[Loop]Fan
[Loop]RotatingObject

[SpeedActive]AeroWing
```

Examples of linked interactions:

```text
[Open_LDoors]LeftDoor
[Open_LDoors]LeftDoorGlass

[Open_RDoors]RightDoor
[Open_RDoors]RightDoorGlass
```

`Group` is optional. `Close` action is optional. 

The `ID` is simply the name of your animation.

For an `Animator`, Interact reads the `AnimationClip` name. For Legacy `Animation`, it reads the `AnimationState` name, which normally matches the source `.anim` clip name.

- Without a group, the clip controls only its own interaction.
- Clips with the same `Group` are triggered together.
- An interaction ID must be unique within its group.
- The same group must contain either `Open`/`Close` interactions or `Loop` interactions; do not mix both types.
- The group name can be arbitrary, just like the ID - they’re simply names/identifiers.

![Docs Interact Mod](https://github.com/Jeefrect/Interact/blob/main/docs/doc-2.png)

---

## Linked groups

A group lets multiple independent animation interactions react to one click. Groups simply link animations together. When one animation is triggered, all other animations in the same group are triggered automatically as well. That’s the only purpose of groups.

Example: two doors that should open and close together:

```text
[Open_Doors]LeftDoor
[Close_Doors]LeftDoor

[Open_Doors]RightDoor
[Close_Doors]RightDoor
```




Clicking either door starts both door animations at the same time. Clicking again closes or reverses both doors.

Each interaction still uses its own animation clip, so clip lengths may differ.

Groups also work with loops:

```text
[Loop_RadiatorFans]LeftFan
[Loop_RadiatorFans]RightFan
```

Clicking either fan starts both loops. Clicking again stops both.

![Docs Interact Mod](https://github.com/Jeefrect/Interact/blob/main/docs/doc-1.png)

---

## Open animation
It refers to an “Open” animation for any object. It can be anything. Example:

```text
[Open]Hood
```

A separate Close animation is not required. If only the Open animation exists, Interact plays that clip in reverse to close its animated target.

> **NOTE**
>
> If an interactive object isn't working for some reason, try adding a **Collider** to it and enabling **Is Trigger**. This is usually not necessary, but it can help in some cases.
>
> If you use a **Mesh Collider** on an object with a dynamic **Rigidbody**, make sure **Convex** is enabled. Unity/PhysX does not support concave Mesh Colliders on dynamic Rigidbody objects.
>
> Alternatively, set the **Rigidbody** to **Is Kinematic** if the object does not require dynamic physics.


## Close animation

To use a separate closing animation, create a matching Close clip:

```text
[Open]DriverDoor
[Close]DriverDoor
```

For grouped clips, both the group and ID must match:

```text
[Open_Doors]LeftDoor
[Close_Doors]LeftDoor
```

## Loop animation

Use `Loop` for animations that continuously repeat while enabled:

```text
[Loop]Fan
[Loop]RotatingObject
[Loop]Wheel
```

Loop interactions work as an on/off toggle:

```text
Click → Start looping
Click again → Stop
```

For grouped loops:

```text
[Loop_Lights]LeftLight
[Loop_Lights]RightLight
```

Clicking the target of either interaction controls the whole `Lights` group.

> **Note:** Interact enables looping for Legacy `Animation` clips at runtime. For `Animator`, Interact restarts the state as needed.

---

## SpeedActive animation

Use `SpeedActive` for animations whose position should automatically follow the vehicle's speed.

Example:

```text
[SpeedActive]ActiveWing
```

The animation timeline represents the full movement range of the object:

```text
First frame  → position at the minimum speed
Last frame   → position at the maximum speed
```

The **minimum speed** setting defines the speed at which the animation is at its **first frame**, while the **maximum speed** defines the speed at which it reaches its **last frame**.

Between these two values, Interact automatically adjusts the animation position according to the current vehicle speed.

For example, with the default range:

```text
Minimum speed: 100 km/h
Maximum speed: 180 km/h
```

the animation position will be:

```text
100 km/h → First frame
140 km/h → Halfway through the animation
180 km/h → Last frame
```

Below the minimum speed, the animation remains at the first frame. Above the maximum speed, it remains at the last frame.

The speed range can be changed in:

```text
Interact Mod → SpeedActive settings
```

> **NOTE**
>
> `SpeedActive` animations are controlled automatically by vehicle speed. They are **not clickable**, cannot be controlled with **F8**, and should **not be assigned to an interaction group**.
>
> `ActiveWing` is only an example ID. You can use any unique ID appropriate for your animation.
---

## NoInteract

Use the `[NoInteract]` prefix on child objects that should **not be clickable** by Interact.

By default, an interaction can use the objects under its interaction root as part of its clickable area. This can cause unrelated child meshes inside the same Kino object to trigger the interaction even though they are not intended to be interactive.

Prefixing a child object with `[NoInteract]` tells Interact to exclude that object from interaction hit detection.

Example:

```text
[Open]DoorCardKinoObject
├── DoorCard_Trim
├── DoorCard_Plastic
├── [NoInteract]Speaker
├── [NoInteract]WindowButtons
└── [NoInteract]DecorativeParts
```

In this example:

```text
DoorCard_Trim       → Clickable
DoorCard_Plastic    → Clickable
Speaker             → Not clickable
WindowButtons       → Not clickable
DecorativeParts     → Not clickable
```

This is useful when a Kino object contains multiple child meshes, but only some of them should be able to trigger the interaction.

> **NOTE**
>
> `[NoInteract]` only affects **Interact's clickable area / hit detection**. It does **not** disable the GameObject, remove its Collider, or prevent Unity animations from affecting it.
>
> If an object marked with `[NoInteract]` is animated by an `AnimationClip`, the animation can still move, rotate, scale, or otherwise modify that object normally. The prefix only prevents that object from being used as a clickable target for the interaction.

For example, an object may still be part of the door animation while being excluded from clicking:

```text
Door
├── DoorMesh
├── Handle
└── [NoInteract]InternalMechanism
```

`InternalMechanism` can still be animated together with the door, but clicking it will not trigger the interaction.

---

## Quick examples

### One interaction with Open only

```text
[Open]HoodAnimation
```

Click once to open; click again to close by reversing the same clip.

### One interaction with Open and Close

```text
[Open]DriverDoorAnimation
[Close]DriverDoorAnimation
```

### Two linked doors

```text
[Open_Doors]LeftDoorAnimation
[Close_Doors]LeftDoorAnimation

[Open_Doors]RightDoorAnimation
[Close_Doors]RightDoorAnimation
```

### Linked doors + mirrors

```text
[Open_LDoor]LeftDoorAnimation
[Close_LDoor]LeftDoorMirrorAnimation

[Open_RDoor]RightDoorAnimation
[Close_RDoor]RightDoorMirrorAnimation
```

### Linked looping fans

```text
[Loop_RadiatorFans]LeftFanAnimation
[Loop_RadiatorFans]RightFanAnimation
```

## Key Binds

You can assign a key or key combination to any interactive object. To create a bind:

1. Hold **Ctrl** and click the interactive object you want to bind.
2. Press the key or key combination you want to assign.
3. Press **Enter** to save.

After saving, the assigned key or key combination can be used to trigger that interaction at any time.

> **NOTE**
>
> **Ctrl** is the default key used to start binding and can be changed in the Interact Mod settings.
>
> Assigning a new bind replaces the previous bind for that interaction.
>
> Press **Esc** to cancel without saving.
