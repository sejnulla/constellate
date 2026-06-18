# Constellate

Browser-based color & object tracking that draws procedural overlays (nodes + connection graphs) on top of video — entirely client-side, no uploads, no backend.

**Live site:** https://sejnulla.github.io/constellate/

## What it does

- **Color mode** — sample one or more colors from the frame; it thresholds in HSV, finds blobs, tracks them frame-to-frame with stable IDs.
- **Smart mode** — runs a YOLO-family object detector (Transformers.js) in a background Web Worker, with a ByteTrack-style two-pass association layer. WebGPU with WASM fallback; quantized weights.
- **Overlays** — connect tracked points with Sequential / Star / Full-mesh / Minimum-spanning-tree / k-NN layouts; straight, curved, or dashed lines.
- **Nodes** — circles or bounding boxes (full rectangle or segmented corner brackets, adjustable border/fill).
- **Styling** — per-target or per-class colors, labels (ID / class / XY coords) with adjustable size, motion trails.
- **Presets** — bundled looks plus save/load your own (localStorage).
- **Export** — composite video + overlay to a `.webm` clip.

## Run locally

It's a single static file. Serve the folder over HTTP (needed so the example videos and the detection worker load):

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

## Example clips

Sample videos live in `examples/` and are selectable from the **Source → Examples** dropdown.

## Tech notes

- Detection runs off the main thread in a module Web Worker, so the UI stays responsive during inference.
- Smart mode requires a modern browser (Chromium/Edge recommended for WebGPU). The first toggle downloads the model (~25 MB) and caches it.
- Tracking eases each box toward its latest detection (no velocity extrapolation), so overlays never drift between sparse detections.

🤖 Built with [Claude Code](https://claude.com/claude-code)
