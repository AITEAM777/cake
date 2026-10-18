# Cake Studio

A self-contained, English-language 3D cake studio inspired by the supplied assorted-cake and utensil references. Application code, styles, procedural textures, geometry, and synthesized audio are in `index.html`.

## Run

Open `index.html` in a modern browser, or serve this directory:

```sh
cd /workspace/cake
python -m http.server 8000 --bind 127.0.0.1
```

There is no build, package installation, backend, or API key. An Internet connection to `cdn.jsdelivr.net` is needed for the pinned Three.js 0.160.1 library, protected by a SHA-384 integrity check. WebGL must be available. The server is only for local development.

## Deploy to Vercel

The production repository branch must contain `index.html`; a local file is not included in a Git-triggered deployment until it is committed and pushed.

Use these project settings:

- Production Branch: `main`.
- Framework Preset: **Other**.
- Root Directory: repository root (leave blank).
- Build Command: empty; this project has no build step.
- Output Directory: `.` (the repository root containing `index.html`).
- Install Command: empty; this project has no package dependencies to install.

After the source is pushed, wait for a successful production deployment. If the existing address still returns 404, check the new deployment's output for `index.html` and confirm the domain points to that production deployment.

## Play

- Drag fully across a slice to cut it. At 100% depth the polygon is split and the layers are rebuilt with closed top, bottom, and cut faces. Shallower passes compress the cake without separating its base.
- Choose one of four knives with distinct timing and crumb behavior, or use the server and fork to lift and gently press pieces.
- Choose Move to select and drag a piece. Use Plate slice for an animated lift onto the serving plate. Placement checks prevent overlapping pieces; make room before serving another large piece.
- Orbit by dragging in Orbit mode; scroll or pinch to zoom. Turntable rotates the cake independently. Top, Side, Macro, and Reset view are available under camera controls.
- Change material response, temperature, messiness, lighting, sound, and time speed in the expandable controls. Decorations, cut topology, and serving positions participate in a 24-step Undo/Redo history.
- Photo mode hides the interface and freezes motion. Orbit to compose the shot, then save a PNG. Before / After temporarily shows the original cake.
- The optional challenges measure an equal-area split, a berry split with modeled cleanliness, or the proportion of pistachio cake successfully plated.

Keyboard: focus the 3D canvas, use arrow keys to position the blade, and Enter to cut. Keys 1–6 choose tools; O enters Orbit, M enters Move, and Escape cancels a drag. Use Tab to reach every panel control; Ctrl/Cmd+Z undoes and Ctrl/Cmd+Shift+Z redoes.

## Simulation scope

Cuts are real, deterministic splits of convex horizontal polygons extruded through every layer, with a 64-piece budget and minimum fragment area. Arbitrary straight vertical cuts and repeated cuts of moved or rotated fragments are supported. This is not unrestricted 3D mesh surgery: tilted, curved, and partial-depth separated cuts are not implemented.

Softness uses bounded spring-driven compression and recovery of each piece. Serving uses constrained motion, polygon collision checks, and a soft landing rather than a full rigid-body solver. Toppings follow their assigned child piece; berries and other individual toppings are not themselves sliced. Crumbs are bounded particles; cleanliness and material response are models, not measurements of real food. The procedural cake follows the photograph's eight flavor identities but is not a photographic reconstruction or an exact recipe.

## Validation

The implementation was checked with Chromium/Playwright, including partial/full and repeated cuts, conserved polygon area and toppings, history, keyboard cutting, serving, camera orbit, pause/speed, audio controls, presets, mobile drawer, reset confirmation, PNG export, automatic demo, decoration, drag cancellation, independent turntable, and all three challenge scores with Undo/Redo. Numerical geometry checks also exercise 180 consecutive splits, degenerate/tangent cuts, coordinate transforms, and triangle winding. Direct mesh checks cover 201 layer meshes, closed oriented faces, volume, and 28 repeated cuts after moving and rotating pieces.
