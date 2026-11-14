# Training Camp

Lets each player train the gesture model on their own body and style.

## Classes

`idle`, `punch_left`, `punch_right`, `block`, `dodge_left`, `dodge_right`, `duck`.

A **class** is one category the model chooses between. `idle` means "no move is happening": it teaches the model that most of the time you are just standing or moving around, which prevents false triggers.

## Recording

- Each move is repeated 8 times on cue (one cue every 2.2 s), then 12 s of free movement.
- Windows ending 0.3 to 0.7 s after a cue are labelled with the move (block: 0.6 to 1.1 s). Calm moments before a cue are labelled `idle`. Windows in between are discarded.

## Features

- A window is 12 samples spaced 40 ms apart (0.44 s), resampled by time so it behaves the same at any frame rate.
- Per sample: 11 body points (nose, ears, shoulders, elbows, wrists, hips) as 3D coordinates relative to the shoulder midpoint and divided by torso length (33 numbers), plus the shoulder midpoint's motion in the image since the window start (2 numbers). 35 features per sample.

## Training

- Held-out validation uses whole repetitions (no leakage from neighbouring windows).
- Augmentation: left/right mirroring (with swapped labels) and small random noise.
- Model: Conv1D(24) -> Conv1D(32) -> Flatten -> Dropout -> Dense(48) -> Softmax(7), about 20k weights. Trained with Adam for 35 epochs on the CPU backend of TensorFlow.js.
- The model and normalisation values are stored in the browser (IndexedDB / localStorage).

## Live use

Probabilities are smoothed. A move fires when its probability passes the confidence slider and it is the top class, then must drop below 0.35 before it can fire again. Block starts after two consecutive high-confidence frames.

## Export format

`shadow-boxer-moves.json`:

```json
{ "classes": [...], "T": 12, "STEP": 40, "F": 35,
  "samples": [ { "c": 1, "rep": 3, "x": [ ...420 numbers... ] } ] }
```

`c` is the class index, `rep` the repetition id (use it to split train/test), `x` the 12 x 35 window flattened.
