# Roadmap

## Next
- Collect much more training data from several people; measure whether accuracy rises with data size.
- Compare AI vs rules on accuracy, false triggers and delay.
- Python notebook: train and evaluate on the exported data, export the best model back to the browser.
- Add moves (hook, uppercut, overhead) as new classes.
- Reduce AI reaction delay (fire earlier in the movement).

## Performance
- Auto-pick GPU or CPU at startup.
- Tracking in a Web Worker; game logic decoupled from tracking FPS.
- Adaptive resolution for weak machines.

## Desktop and Blender (separate system-level app)
- Desktop mode: hand tracking (pinch to click, drag, scroll, swipe), engage/disengage gesture, optional voice commands and coarse eye tracking.
- A Python background app with a tray icon that creates a virtual mouse and keyboard. See [../desktop/README.md](../desktop/README.md).
- Blender add-on that receives the same events.
- Modes: Game (body), Desktop (hands, face, voice), Blender (hands, voice). One tracking model at a time.

## Multiplayer idea
Send gesture events (not video) between two players over WebRTC or a relay server.
