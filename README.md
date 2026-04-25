# CS 330 — 3D Scene
 
**Category:** Software Design and Engineering
**Original course:** CS 330 — Computational Graphics and Visualization (September 2024)

## What the original artifact is

A C++/OpenGL desktop program that renders a 3D castle scene. It uses GLSL shaders, texture mapping (marble, copper, gold, sky), multiple light sources, and keyboard/mouse camera controls. Around 2,700 lines across `SceneManager.cpp`, `ViewManager.cpp`, `MainCode.cpp`, and shader files. Runs on Windows only and needs Visual Studio plus the OpenGL/GLFW/GLEW stack to build.

## Why I chose it

The scene itself was fine, but the delivery story was bad. You could only see it by cloning the repo, installing a Windows toolchain, and building it yourself. That's not a portfolio piece, that's a homework screenshot. Porting it to the web makes it something anyone can open.

## What I enhanced

Ported the entire scene from C++/OpenGL to **Three.js/WebGL** in a single self-contained HTML file. No build step, no install, just open it in a browser.

- Cylinders, spires, planes, and textures all rebuilt in Three.js using `CylinderGeometry`, `ConeGeometry`, `PlaneGeometry`, and procedural canvas textures for marble, copper, gold, and sky (keeps the file self-contained — no external image hosting)
- Broke the monolithic `SceneManager` into smaller functions for geometry creation, texture generation, lighting setup, and the render loop
- Added **OrbitControls** for camera movement and a **dat.GUI** panel for live lighting adjustments
- Shadow mapping and tone mapping for a less flat-looking scene
- Responsive canvas that resizes with the browser window

## Course outcomes this hits

- **Outcome 2 (professional communications):** a visual deliverable anyone can open and interact with, plus this narrative explaining what changed and why
- **Outcome 3 (algorithmic principles and trade-offs):** choosing WebGL over native OpenGL trades some raw performance for near-universal accessibility, which for a portfolio piece is the right call
- **Outcome 4 (well-founded, innovative tools):** Three.js is the de facto standard for 3D on the web, and using it shows I can pick up and ship with a current framework

## Files

- `README.md` — this narrative
- `enhanced_scene_Daniel_Collins.html` — the Three.js port (open in a browser)
- `CS499_Milestone_Two_Daniel_Collins.zip` — full milestone submission with enhanced artifact and `originals/` folder containing the unmodified C++ source
- Original C++ source files and prior milestone zips are preserved for reference

## How to view it

Just open `enhanced_scene_Daniel_Collins.html` in any modern browser. No server needed.

---

Part of my CS 499 ePortfolio — [dcollins123.github.io](https://dcollins123.github.io)
