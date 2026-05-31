# REBOUND — AR Wall Trainer

Turn any wall + projector into an interactive football target game. A camera
watches the wall, detects your ball, and scores hits on projected targets by
**accuracy, speed, and difficulty**.

**Status:** v1 — colour/motion ball tracking, 4-corner calibration, accuracy +
speed + streak scoring, particle hit feedback, offline WebAudio sound. No
dependencies, fully self-contained.

---

## How it works

1. A camera (iPhone via Continuity Camera, or any webcam) points at the projected wall.
2. You calibrate by clicking the 4 projected corners — this builds a perspective
   transform (homography) mapping camera pixels to game coordinates.
3. Each frame the ball is found (bright-colour tracking or motion) and mapped into game space.
4. Targets appear; ball-in-target = points, scaled by how centred the hit is,
   how fast it landed, and the difficulty + current streak.

## Requirements

- A projector (e.g. Anker Nebula) on a plain wall.
- A camera pointed at the wall — iPhone (Continuity Camera on a Mac) or any USB / built-in webcam.
- A **solid, bright-coloured ball** (orange / yellow / pink track best; avoid the classic white-and-black).

## Run locally

The camera is blocked on `file://`, so serve over https or localhost:

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

## Setup (in the app)

Open the ⚙ panel:

1. **Start Camera**, then pick your iPhone / webcam from *Source*.
2. **Calibrate** — press `F` for fullscreen first, then click the 4 projected corners in order: **TL → TR → BR → BL**.
3. **Ball** — pick the colour, or *Sample* it by clicking the ball in the preview.
4. **Play** — pick difficulty and press `Space`.

**Keys:** `C` controls · `D` detection mask · `F` fullscreen · `Space` start/stop.

## Tuning

- Turn on the mask (`D`) and adjust **Sensitivity** / **Min ball size** until only the ball lights up green.
- Sample the ball colour under the *actual* playing light; keep some ambient light in the room.
- Calibrate after the projection is at its final size; re-click the corners if you resize the window or change displays.

## ⚠️ Real ball, real gear

A kicked ball — especially power shots — will destroy a phone or projector on a
direct hit. Mount the camera and projector **high and off to one side** (the
calibration corrects for moderate angles), out of the rebound path.

## Roadmap (v2)

- 60fps capture request + power-shot **trajectory tracking** (catch fast shots that blur between frames).
- **Kick-up counter** (sustained airborne touches).
- Swap colour-blob tracking for a trained detector (TensorFlow.js / YOLO) for robustness under messy light.

## Deploy to Vercel

This repo is static — no build step. Import it in Vercel (**Add New → Project →
Import**), framework preset **Other**, and deploy. Every push auto-deploys.

---

Built with vanilla JS + Canvas.
