# AirLuxe — Smart Blackboard ✋✏️

Draw on a virtual blackboard with hand gestures — no touch screen needed. Built as a single
`index.html`, tuned to run on **low-end devices with poor webcams**.

**Live demo:** https://shi22khar-oss.github.io/AirLuxe/

## Gestures

| Gesture | Action |
| --- | --- |
| ☝️ Index finger only | Draw |
| ✌️ Index + middle | Erase |
| ✋ Open palm (hold ~1 s) | Clear board |
| 🤏 Pinch thumb + index | Cycle chalk color |

Keyboard shortcuts: `1–5` colors · `C` clear · `H` hint · `M` camera preview · `G` settings.
If the camera is unavailable, the app automatically falls back to mouse/touch
(click-drag = draw, right-drag or two-finger touch = erase).

## Optimizations for low-end devices & poor cameras

- **Adaptive performance modes** (`Auto` / `Low-end` / `Balanced` / `High`): the camera feed
  (320×240 → 640×480), tracking rate (≈8–18 fps), MediaPipe model complexity and canvas
  resolution auto-adjust to the device; `Auto` up/down-grades live based on measured
  processing time.
- **Low-light auto-enhancement**: frame luminance is sampled and dim frames are
  brightness/contrast-boosted (LUT-based, cheap) before hand tracking — grainy dark webcams
  track far better.
- **Cheap rendering**: no `shadowBlur`, no `backdrop-filter`; glow is layered strokes;
  effects and animations disable automatically in low-end mode.
- **Robust tracking**: lenient detection thresholds, One-Euro adaptive smoothing +
  deadzone + velocity prediction, gesture hysteresis (debounced classification),
  outlier-jump rejection, tip/DIP pen blending, and stroke continuity across brief
  tracking dropouts.
- **Feedback**: live FPS/processing-time/resolution meter and low-light hints.
- Settings persist in `localStorage`; MediaPipe is loaded from a CDN with a fallback mirror.

## Deployment (GitHub Pages)

The live site is published from the repo root at
https://shi22khar-oss.github.io/AirLuxe/ (GitHub Pages, `main` branch).
Changes are landed via pull request — merging a PR to `main` redeploys automatically.

An optional Actions-based pipeline is also included at
`.github/workflows/deploy-pages.yml`; to use it, set the Pages source to
**GitHub Actions** in the repo settings (Settings → Pages).

## Local development

Just serve the folder over HTTPS (camera access requires a secure context):

```bash
python3 -m http.server 8000   # then open https://.../ (or use GitHub Pages / any HTTPS host)
```
