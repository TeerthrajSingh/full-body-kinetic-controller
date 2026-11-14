# Full-Body Kinetic Controller

A zero-controller human-computer interface: your webcam sees you, an AI model figures out what you are doing, and that becomes input for games and (later) your whole desktop. No gloves, no sensors, no controller.

The first showcase app is **Shadow Boxer**: a boxing game where you punch, block, dodge and duck in front of your camera to fight an AI opponent.

> Hackathon project. Status: working prototype (see [Status](#status) and [Known limitations](#known-limitations)).

---

## Features

- **Real-time body tracking** in the browser with Google MediaPipe Pose Landmarker (runs locally, no video leaves your computer).
- **Smoothing** with a One Euro filter to remove landmark jitter.
- **Two gesture recognizers you can switch between:**
  - **Rules**: hand-written geometric checks (arm length, elbow angle, speed).
  - **AI model**: a small neural network that you train yourself in the **Training Camp** by recording your own moves.
- **Event-based design:** the vision code only emits events (`punch`, `block_start`, `dodge`, ...). Games and tools just listen, so new apps can be added without touching the tracker. See [docs/GESTURE_EVENTS.md](docs/GESTURE_EVENTS.md).
- **Shadow Boxer game:** 3 AI opponents with attack wind-ups, blocking, dodging, counters, stamina, combos, sound effects and a keyboard fallback.

## Quick start

You need **Chrome or Edge**, a **webcam**, and an **internet connection** (the pose model and libraries load from CDNs).

The camera only works on `localhost` or HTTPS, so run a tiny local server:

```bash
git clone https://github.com/TeerthrajSingh/full-body-kinetic-controller
cd full-body-kinetic-controller/web
python3 -m http.server 8000
```

Open <http://localhost:8000/index.html>, then:

1. Click **Start camera** and allow camera access.
2. Click **Calibrate** and stand in your fighting stance when the countdown ends.
3. Click **Fight!**

No camera? Use the keyboard: `J` / `K` punch left/right, `B` toggle block, `A` / `D` dodge left/right, `S` duck.

### Setup tips

- Stand 1.5 to 2 m from the camera so your head, shoulders and hips are all visible.
- Light yourself from the front (not backlit). Plug in a laptop; many slow down on battery.
- Stand at about 45 degrees to the camera (a boxer's stance). Punches that travel across the image are tracked much better than punches pointed straight at the lens (see [Known limitations](#known-limitations)).

## How to play

The opponent winds up with a shrinking ring and an on-screen prompt, then strikes.

| Attack | How to defend |
|---|---|
| Hook from the left | Dodge right, or duck |
| Hook from the right | Dodge left, or duck |
| Straight punch | Block, or dodge either way |
| Overhead | Duck |

Your punches do more damage as counters (while the opponent recovers). Chain hits for a combo bonus. Punches cost stamina. Win by knockout or by having more health when the 2 minutes end. There are three opponents, each faster and tougher.

## Training Camp (train the AI on your own moves)

1. Start the camera, then click **Training Camp** in the side panel.
2. Follow the on-screen coach: each move is repeated 8 times on cue, then you move freely for 12 seconds.
3. A small neural network trains in your browser (seconds). The panel shows held-out accuracy and a confusion table.
4. Switch the recognizer to **AI model**. The model is saved in your browser, so you only train once.
5. **Export data** saves your recordings as `shadow-boxer-moves.json` for offline analysis.

Details: [docs/TRAINING_CAMP.md](docs/TRAINING_CAMP.md).

## How it works

```
Webcam -> MediaPipe Pose -> One Euro filter -> Recognizer (rules OR AI) -> "gesture" events -> Game
```

More in [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## Project milestones

| # | Milestone | Status |
|---|---|---|
| 1 | Webcam pipeline + MediaPipe landmarks | Done |
| 2 | Temporal smoothing (One Euro) | Done |
| 3 | Gesture engine: heuristics, then a trained sequence classifier | Heuristics done; classifier prototype done, needs more data |
| 4 | Low-latency OS input events (< 100 ms), connect to a target app | Not started (desktop helper planned, see [desktop/](desktop/)) |
| 5 | Polished HUD, tuning, recorded demo | HUD done; tuning and demo in progress |

## Status

Working prototype. Roadmap: [docs/ROADMAP.md](docs/ROADMAP.md).

## Known limitations

- **Depth:** a webcam is 2D, so punches straight at the camera are often missed. Punching across the image works well.
- **The AI model is not yet better than the rules.** We believe the main reason is very little training data (about 8 repetitions per move, from one person). This is a hypothesis we plan to test with more data from several people.
- The AI model reacts after about half a second of movement, so it can feel later than the rules.
- Not yet tested on low-end machines beyond the development laptop (Intel UHD 620 integrated graphics, about 21 FPS tracking).
- Only these moves exist so far: left/right punch, block, dodge left/right, duck.

## Tech stack

- [MediaPipe Tasks Vision](https://developers.google.com/mediapipe) (`@mediapipe/tasks-vision` 0.10.14), Pose Landmarker lite model
- [TensorFlow.js](https://www.tensorflow.org/js) 4.22.0 for in-browser training and inference
- Plain HTML, CSS and JavaScript (no build step), Canvas 2D for the game, Web Audio for sound

## Troubleshooting

See [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md).

## Repository layout

```
web/index.html         Shadow Boxer game + tracker + Training Camp (single file)
web/tracker-only.html  Tracker and gesture event log only, no game
docs/                  Architecture, event reference, Training Camp, roadmap, troubleshooting
desktop/               Planned Python desktop controller (not yet implemented)
```

## License

MIT, see [LICENSE](LICENSE).

## Acknowledgements

Google MediaPipe and TensorFlow.js teams.
