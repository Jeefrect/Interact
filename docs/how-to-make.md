# Interact Animation Guide

This guide describes how to make Kino custom objects compatible with Interact. 

The mod works with KINO custom objects and KINO car parts only!

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

A group lets multiple independent animation interactions react to one click.

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

The Open animation must go from the closed state to the fully open state:

```text
First frame → Closed
Last frame  → Open
```

Example:

```text
[Open]Hood
```

A separate Close animation is not required. If only the Open animation exists, Interact plays that clip in reverse to close its animated target.


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

## Quick examples

### One interaction with Open only

```text
[Open]Hood
```

Click once to open; click again to close by reversing the same clip.

### One interaction with Open and Close

```text
[Open]DriverDoor
[Close]DriverDoor
```

### Two linked doors

```text
[Open_Doors]LeftDoor
[Close_Doors]LeftDoor

[Open_Doors]RightDoor
[Close_Doors]RightDoor
```

### Linked looping fans

```text
[Loop_RadiatorFans]LeftFan
[Loop_RadiatorFans]RightFan
```
