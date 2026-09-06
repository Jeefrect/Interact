## Prevent animations from playing automatically

If an animation briefly plays when a KINO object is loaded, the animation is most likely configured as the default state in its **Animator Controller**.

Interact disables automatic animation playback after the object has fully loaded, but the Animator may start its default animation before Interact takes control.

To prevent this:

1. Open the object's **Animator Controller**.
2. Find the current **Default State** (shown in orange).
3. Create a new "empty" state.
4. Right-click `New state` and select **Set as Layer Default State**.
6. Done.

Do not use it for loop animations.
