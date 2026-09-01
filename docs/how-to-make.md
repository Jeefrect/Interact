# Interact Animation Guide

This guide describes how to make Kino custom objects compatible with Interact.

## Clip naming

Animation clips must use the following format (name):

```text
Interact_<Action>_<ID>
```

Supported actions:

```text
Open
Close
Loop
```

Examples:

```text
Interact_Open_DriverDoor
Interact_Close_DriverDoor

Interact_Open_Rear_Left_Door
Interact_Close_Rear_Left_Door

Interact_Open_Hood
Interact_Open_Trunk

Interact_Loop_Fan
Interact_Loop_RotatingObject
```

The `ID` identifies the interactive object and may contain underscores.

Each interaction ID must be unique within the car.

---

## Open animation

The Open animation must go from the closed state to the fully open state:

```text
First frame → Closed
Last frame  → Open
```

Example:

```text
Interact_Open_Hood
```

A separate Close animation is not required.

If only the Open animation exists, Interact automatically uses it in reverse to close the object.

### Loop setting

It is recommended to keep the `Loop` option disabled on Open animation clips.

Open animations are expected to play once and stop at their final state.

> **Note:** Interact automatically disables looping for `Open` animations at runtime, regardless of the animation clip's Loop setting.

---

## Close animation

If you want to use a separate closing animation, create:

```text
Interact_Close_<ID>
```

The Close animation must go from the fully open state back to the closed state:

```text
First frame → Open
Last frame  → Closed
```

Example pair:

```text
Interact_Open_DriverDoor
Interact_Close_DriverDoor
```

The final pose of `Open` and the initial pose of `Close` must match.

### Loop setting

It is recommended to keep the `Loop` option disabled on Close animation clips.

> **Note:** Interact automatically disables looping for `Close` animations at runtime, regardless of the animation clip's Loop setting.

---

## Loop animation

Use the `Loop` action for animations that should continuously repeat while enabled.

Format:

```text
Interact_Loop_<ID>
```

Examples:

```text
Interact_Loop_Fan
Interact_Loop_RotatingObject
Interact_Loop_Wheel
```

Loop interactions work as an on/off toggle:

```text
Click → Start looping
Click again → Stop
```

This is intended for continuously animated objects such as rotating or moving parts.

It is recommended to enable the `Loop` option on Loop animation clips.

> **Note:** Interact automatically enables looping for `Loop` animations at runtime, regardless of the animation clip's Loop setting.

Because of this, manually setting the Loop option correctly is recommended for consistency, but is not required for Interact to work.

---

## Animator setup

If you use an `Animator`, layer `0` must contain a state whose name exactly matches the animation clip name.

Example:

```text
Clip:
Interact_Open_DriverDoor

State:
Interact_Open_DriverDoor
```

The same rule applies to `Open`, `Close`, and `Loop` animations.

Example:

```text
Clip:
Interact_Loop_Fan

State:
Interact_Loop_Fan
```

---

## Object selection

The Kino object must have at least one of the following on itself or its children:

* `Collider`
* enabled `Renderer`

This allows Interact to detect the object when the player points at it.

---

## Quick examples

### Open / Close

A door with a single animation:

```text
Interact_Open_DriverDoor
```

is enough to support opening and closing.

For separate opening and closing animations:

```text
Interact_Open_DriverDoor
Interact_Close_DriverDoor
```

Both clips use the same interaction ID:

```text
DriverDoor
```

### Loop

For a continuously animated object:

```text
Interact_Loop_Fan
```

The first interaction starts the animation and the next interaction stops it.
