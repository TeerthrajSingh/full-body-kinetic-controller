# Desktop controller (planned, not yet implemented)

A system-level Python app that turns hand gestures (and later face and voice) into real mouse and keyboard input, plus a Blender add-on. It must run as a background feature on your computer, not as a web page.

## Planned design

- Python 3.12 (MediaPipe's pip packages do not support very new Python versions; use 3.11 or 3.12).
- Camera capture thread, MediaPipe Hand Landmarker, One Euro filter, gesture recognizer, tray icon, mode switching.
- Input backend: `uinput` / `ydotool` on Linux (works on Wayland and X11), with swappable backends for Windows and macOS.
- Safety: an engage/disengage gesture and a global kill switch.

## Development environment (Linux)

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh     # then open a new terminal
cd desktop
uv venv --python 3.12
source .venv/bin/activate
uv pip install mediapipe opencv-python numpy
python --version
```
