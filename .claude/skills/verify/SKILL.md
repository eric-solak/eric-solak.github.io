---
name: verify
description: Verify changes to this static portfolio site (index.html + nn-classifier map) by serving locally and driving headless Edge with playwright-core.
---

# Verify this site

Static site — no build step. Serve and drive a real browser:

1. Serve: `py -m http.server 8741 --bind 127.0.0.1` from the repo root (background).
2. Driver: `npm install playwright-core` in a scratch dir, then
   `chromium.launch({ channel: 'msedge', headless: true })` — uses system Edge,
   no browser download needed.
3. Flows worth driving:
   - `index.html`: page-weight audit via `page.on('response')`; the Pocket AI
     modal (`.proj-tile-phone` click → `#modal-video` should only start loading
     then — it has `preload="none"` and no autoplay; Escape must close + pause);
     scroll through sections so IntersectionObserver animations fire.
   - `nn-classifier/wildfire-risk-map.html`: wait ~2.5s for CSV plot; legend
     counts `#cnt-0/1/2` populate; drag hard N/S/E/W — map must clamp with no
     gray void (viewport-aware min-zoom means `.leaflet-control-zoom-out` is
     disabled at world view); hover a cluster → `.info-box` shows "Confidence";
     click locks the panel.

Gotchas:
- PowerShell 5.1: no `&&` — use Git Bash tool for chained commands.
- `assets/img/sneakpeek-preview.png` intentionally 404s (onerror hides the tile).
- Images are pre-optimized; if replacing, keep filenames (og:image points at
  `assets/img/me.jpg`) and use ffmpeg (installed) for resize/webp/x264.
