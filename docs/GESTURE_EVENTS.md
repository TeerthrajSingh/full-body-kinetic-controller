# Gesture event reference

Every recognizer (rules, AI, keyboard) emits the same events on `window`:

```js
window.addEventListener("gesture", (e) => console.log(e.detail));
```

`detail` fields:

| Field | Meaning |
|---|---|
| `type` | `"punch"`, `"block_start"`, `"block_end"`, `"dodge"`, `"duck"` |
| `hand` | `"left"` or `"right"` (punch only) |
| `dir` | `"left"` or `"right"` (dodge only) |
| `power` | 0 to 1 (punch only) |
| `t` | `performance.now()` timestamp when emitted |

`hand` and `dir` are from the **player's** point of view and match the mirrored on-screen camera view.

## Keyboard fallback

| Key | Event |
|---|---|
| `J` / `K` | punch left / right |
| `B` | toggle block |
| `A` / `D` | dodge left / right |
| `S` | duck |

## Using the events in another app

Load the tracker, listen for `gesture`, and ignore the rest. The game in `web/index.html` is a complete example.
