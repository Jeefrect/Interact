# Interact Animation Guide

This guide describes how to make Kino custom objects compatible with Interact.

## Clip naming

Animation clips must use the following format:

```text
Interact_<Action>_<ID>
```

Supported actions:

```text
Open
Close
```

Examples:

```text
Interact_Open_DriverDoor
Interact_Close_DriverDoor

Interact_Open_Rear_Left_Door
Interact_Close_Rear_Left_Door

Interact_Open_Hood
Interact_Open_Trunk
```

`Interact_Open_<ID>` is required.

`Interact_Close_<ID>` is optional.

The `ID` identifies the interactive object and may contain underscores.

Open and Close animations for the same object must use exactly the same ID:

```text
Interact_Open_DriverDoor
Interact_Close_DriverDoor
```

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

The same applies to the Close animation.


## Object selection

The Kino object must have at least one of the following on itself or its children:

* `Collider`
* enabled `Renderer`

This allows Interact to detect the object when the player points at it.

---

## Quick example

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
