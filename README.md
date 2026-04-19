# Drowsy Detector (MediaPipe)

Browser-based drowsiness detector using MediaPipe FaceLandmarker and Eye Aspect Ratio (EAR). Runs entirely client-side — no server, no API calls.

## Demo

1. Clone and serve:
   ```bash
   git clone https://github.com/whatfontisthis/drowsy-detector-mediapipe.git
   cd drowsy-detector-mediapipe
   python3 -m http.server 8000
   ```
2. Open `http://localhost:8000`
3. Click **Start Camera**, allow webcam.

Requires localhost or HTTPS (`getUserMedia` restriction).

## How it works

See [FLOW.md](FLOW.md) for full diagram.

- MediaPipe FaceLandmarker returns 478 face landmarks per frame.
- EAR formula on 6 eye landmarks measures openness:
  `EAR = (|p2-p6| + |p3-p5|) / (2 * |p1-p4|)`
- Open eye ≈ 0.3, closed ≈ 0.1. Threshold `< 0.22` sustained for 1.5s triggers alert.
- Alert: red flashing status + 880Hz beep via WebAudio.

## Tuning

Edit [app.js](app.js):
- `EAR_THRESHOLD` — lower = less sensitive
- `DROWSY_MS` — how long eyes must stay closed

## Stack

- MediaPipe Tasks Vision (WASM + GPU delegate via WebGL)
- Vanilla HTML/JS, no build step
- CDN for MediaPipe runtime, local `face_landmarker.task` model (3.6MB)

## License

MIT
