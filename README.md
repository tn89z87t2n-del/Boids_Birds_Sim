# 🐦 Boids Birds Sim — Murmuration

A mesmerizing, large-scale flocking simulation of **Craig Reynolds' boids**, built as a **single self-contained `index.html`** (inline CSS + JS, zero dependencies). Thousands of birds flow like liquid — a real murmuration of starlings.

**▶ Live demo:** open [`index.html`](index.html) locally, or view it instantly here:
- https://raw.githack.com/tn89z87t2n-del/Boids_Birds_Sim/main/index.html
- https://htmlpreview.github.io/?https://github.com/tn89z87t2n-del/Boids_Birds_Sim/blob/main/index.html

![boids](https://img.shields.io/badge/boids-up%20to%2020%2C000-8fd6ff) ![webgl](https://img.shields.io/badge/render-WebGL%20instanced-orange) ![deps](https://img.shields.io/badge/dependencies-none-green)

## ✨ Features

### Simulation — the 3 classic rules
- **Separation** — steer away from neighbors that are too close (short radius)
- **Alignment** — steer toward the average heading of neighbors (medium radius)
- **Cohesion** — steer toward the center of mass of neighbors (medium radius)
- Each rule has its own adjustable **radius and weight** (UI sliders)
- Proper **Reynolds steering** (`steer = desired − velocity`, clamped to max force) — this is what makes the motion look natural
- Max speed + min speed clamping, slight random jitter
- **Soft world bounds** — flocks turn back gracefully, no hard bouncing
- **🦅 Predator mode** — your mouse cursor (or touch) is a hawk; boids flee with high priority, creating the spectacular splitting/reforming murmuration effect
- **🌬 Wind** — optional slowly rotating global force vector (subtle)

### Scale & performance
- **5,000 boids at 60 fps**, slider up to **20,000** (core update step measured at ~2.9 ms @ 5k, ~12 ms @ 20k)
- **Spatial hash grid** for neighbor lookups (counting sort, no O(n²)) — rebuilt each frame into reused arrays, zero per-frame allocations / GC churn
- All boid state in flat **`Float32Array`s** (SoA), no objects
- **WebGL instanced rendering** — one draw call, each boid is a small elongated triangle oriented along its velocity (WebGL2, falls back to WebGL1 + `ANGLE_instanced_arrays`)
- Neighbor sampling cap: each boid tracks max **12 nearest neighbors** (real starlings track ~7)

### Visuals
- Dark gradient sky (deep navy → dusk orange horizon with a sun glow)
- Color modes: **local density** (sparse cyan → dense warm white), **speed**, or **direction hue**
- Additive blending — dense clusters glow brighter
- Subtle **motion blur** trails (adjustable, frame-fade instead of full clear)
- Camera: **wheel zoom** + **right-drag pan** (pinch/two-finger on touch)

### UI
- Dark glassmorphism panel: boid count, 3 rule weights + radii, max speed, trail amount, flee radius, predator & wind toggles, color mode, pause/reset
- **Presets:** `Murmuration` · `Loose flock` · `Panic` · `Rivers`
- FPS counter + live boid count

## ⌨️ Controls

| Key / input | Action |
|---|---|
| `Space` | Pause / resume |
| `P` | Toggle predator |
| `R` | Reset flock + camera |
| `H` | Hide / show UI |
| `1`–`4` | Presets |
| Mouse move | Predator (hawk) position |
| Mouse wheel | Zoom (toward cursor) |
| Right-drag | Pan camera |
| 1 finger / 2 fingers (touch) | Predator / pan + pinch zoom |

## 🛠 Code structure

Everything lives in `index.html`, organized in clearly commented sections (comments in Slovak, identifiers in English):

1. **Boid state** — flat `Float32Array`s
2. **Spatial hash grid** — counting sort into preallocated arrays
3. **Boid update** — the 3 rules clearly separated, predator, wind, soft bounds, integration
4. **WebGL rendering** — instanced triangles + gradient sky with trail fade
5. **UI & input** — sliders, presets, camera, keyboard

Just open `index.html` in any modern browser. No build step, no server, no dependencies.
