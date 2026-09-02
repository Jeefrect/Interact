# Interact Animation Guide

This guide describes how to make Kino custom objects compatible with Interact.

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

`Group` is optional.

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

Examples of linked objects:

```text
[Open_Doors]LeftDoor
[Close_Doors]LeftDoor

[Open_Doors]RightDoor
[Close_Doors]RightDoor
```

The `ID` identifies the clickable object and may contain underscores.

- Without a group, the interaction affects only its own object.
- Objects with the same `Group` are switched together.
- An object ID must be unique within its group.
- The same group must contain either `Open`/`Close` interactions or `Loop` interactions; do not mix both types.

The old format is still supported for compatibility, but does not support groups:

```text
Interact_Open_Hood
Interact_Close_Hood
Interact_Loop_Fan
```

---

## Linked groups

A group lets multiple independent objects react to one click.

Example: two doors that should open and close together:

```text
[Open_Doors]LeftDoor
[Close_Doors]LeftDoor

[Open_Doors]RightDoor
[Close_Doors]RightDoor
```

Clicking either door starts both door animations at the same time. Clicking again closes or reverses both doors.

Each object still uses its own animation clip, so animation lengths may differ.

Groups also work with loops:

```text
[Loop_RadiatorFans]LeftFan
[Loop_RadiatorFans]RightFan
```

Clicking either fan starts both loops. Clicking again stops both.

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

A separate Close animation is not required. If only the Open animation exists, Interact plays it in reverse to close the object.

### Loop setting

Keep the `Loop` option disabled for Open clips.

> **Note:** Interact forces Open clips to play once at runtime, regardless of the source clip's Loop setting.

---

## Close animation

To use a separate closing animation, create a matching Close clip:

```text
[Open]DriverDoor
[Close]DriverDoor
```

For grouped objects, both the group and ID must match:

```text
[Open_Doors]LeftDoor
[Close_Doors]LeftDoor
```

The Close animation must go from the fully open state back to the closed state:

```text
First frame → Open
Last frame  → Closed
```

The last pose of Open and the first pose of Close must match.

### Loop setting

Keep the `Loop` option disabled for Close clips.

> **Note:** Interact forces Close clips to play once at runtime, regardless of the source clip's Loop setting.

---

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

Clicking either object controls the whole `Lights` group.

> **Note:** Interact enables looping for Legacy `Animation` clips at runtime. For `Animator`, Interact restarts the state as needed.

---

## Animator setup

If using an `Animator`, layer `0` must contain a state whose name exactly matches the clip name.

Example:

```text
Clip:
[Open]DriverDoor

State:
[Open]DriverDoor
```

Grouped example:

```text
Clip:
[Open_Doors]LeftDoor

State:
[Open_Doors]LeftDoor
```

The same rule applies to Open, Close, and Loop animations.

---

## Object selection

The Kino object must have at least one of the following on itself or its children:

- `Collider` — trigger mode is recommended.
- Enabled `Renderer`.

This allows Interact to detect the object when the player points at it.

---

## Quick examples

### One object with Open only

```text
[Open]Hood
```

Click once to open; click again to close by reversing the same clip.

### One object with Open and Close

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
