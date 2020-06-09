# Input

The Input module provides a way to map `InputEvent`s to an event or message type that drives your game.

## Configuration

The plan is to have configuration through JSON, of the following format:
```
{
  "inputs": {
    "escape": "Quit",
    "w": "MoveUp",
    "s": "MoveDown,
    "lmb": "Pause",
    "ctrl": "Crouch",
    "crtl up": "Uncrouch"
  }
}
```

The keys to the map should be identifiers for keys, or `lmb`/`rmb`, or `mouse-motion` or `mouse-wheel`. Simple controller supported is planned for the future. The event type for `mouse-wheel` and `mouse-motion` should except to receive an argument of one and two ints, respectively: for `mousewheel`, the `delta` of the wheel's axis, and for `mouse-motion`, the new `x` and `y` coordinates.

## Initialization

Initialize your bindings with `create-bindings-from-json` or, for hard-coded bindings, `create-bindings-from-array`. Store these bindings in your global state. 

## Loop
In your event handler, you can send SDL Events directly to `value-by-event`, which will then return whatever kind of game event you specified in your configuration, and which you can dispose of as you please.
