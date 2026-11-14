# Architecture

The vision pipeline is kept separate from the apps that use it. The tracker produces events; applications only listen.

```
Webcam
  -> MediaPipe Pose Landmarker (33 landmarks: image coords + metric 3D "world" coords)
  -> One Euro filter (smooths each landmark; strong smoothing when still, quick when moving)
  -> Recognizer (switchable)
       Rules: arm length, reach, speed, elbow angle, state machines
       AI:    1D-CNN over a 0.44 s window of body features
  -> window "gesture" CustomEvent
  -> Game (Shadow Boxer) / future: desktop helper, Blender add-on
```

## Rule-based recognizer (summary)

- Distances are in body-relative units (arm lengths, torso lengths) using MediaPipe world landmarks, so results do not depend on camera distance.
- **Punch:** the wrist extends away from the shoulder faster than a threshold, travels far enough, and ends fairly straight. Per-hand cooldown, a retract-before-next-punch rule, and a 400 ms cooldown across both hands stop one swing from counting twice.
- **Block:** both wrists up near the face (with hysteresis).
- **Dodge / duck:** the head position in the image is compared with the calibrated neutral spot (in shoulder widths). Image space is used because hip-relative height cannot see a squat.

## AI recognizer (summary)

See [TRAINING_CAMP.md](TRAINING_CAMP.md).

## Performance notes

- Tracking and game rendering run on the same thread, so inference time (about 42 ms on the development laptop) affects smoothness. Planned: run tracking in a Web Worker and decouple the game from the tracking frame rate.
- The Delegate dropdown (GPU/CPU) and Model dropdown (lite/full) let you trade speed against accuracy.
