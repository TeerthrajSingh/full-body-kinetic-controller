# Troubleshooting

| Problem | Fix |
|---|---|
| Camera does not start | Open the page from `http://localhost:8000`, not by double-clicking the file. Allow camera access. Close other apps using the camera (Zoom, Teams). |
| `error: ...` after Start camera | Switch Delegate to CPU and try again. Check your internet connection (model and libraries load from CDNs). |
| Page behaves like an old version | Hard refresh with Ctrl+Shift+R. |
| Low FPS | Compare the GPU and CPU delegates (FPS and ms are shown). Keep the lite model, plug in the laptop, improve lighting. |
| Punches not detected | Stand at about 45 degrees to the camera, lower the Punch sensitivity slider, make sure your whole upper body is visible. |
| One punch counts twice | Raise `punchGlobalCooldownMs` or `rearmDrop` in the `cfg` block of `web/index.html`. |
| Ducks not detected | Calibrate again, lower the Duck depth slider. |
| Training Camp says "Not enough data" | Your upper body was not fully visible during recording. Step back and retry. |
| AI model worse than rules | Expected with little data. Record more repetitions, then compare. |

## Tuning values (`cfg` in `web/index.html`)

| Setting | Meaning |
|---|---|
| `punchVel` | Speed needed to start a punch (arm-lengths per second). Lower is easier. |
| `minReach`, `minElbowAngle`, `minTravel` | How far and how straight the punch must end. |
| `punchGlobalCooldownMs` | Pause after any punch before another can fire. |
| `rearmDrop` | How far the hand must pull back before it can punch again. |
| `dodge`, `duckDrop` | Head movement (in shoulder widths) needed for a dodge or duck. |
