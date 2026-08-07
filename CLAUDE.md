# CLAUDE.md

Guidance for AI assistants (Claude Code and similar) working in this repository.

## What this repository is

This is a **portfolio / coursework archive**, not a conventional software project.
It holds the deliverables for a 3D castle scene that began as a CS 330 (Computational
Graphics and Visualization) assignment and was later enhanced for a CS 499 Computer
Science Capstone ePortfolio.

There are two distinct artifacts, and it matters which one a request is about:

1. **Original artifact — C++/OpenGL desktop app.** A Windows-only Visual Studio
   program (~2,700 lines) that renders a castle scene using GLSL shaders, texture
   mapping, and multiple lights. Source: `SceneManager.cpp`, `ViewManager.cpp`,
   `MainCode.cpp`, and `shaders/*.glsl`. This code is **frozen reference material** —
   it is preserved as history, not actively developed here.

2. **Enhanced artifact — Three.js/WebGL web port (the live, active work).** A single
   self-contained HTML file, `enhanced_scene_Daniel_Collins.html`, that reimplements
   the same scene so anyone can open it in a browser with no build step or install.
   **This is the file to edit for any "improve / fix / extend the scene" request.**

`README.md` is the CS 499 narrative explaining the enhancement and course outcomes.

## Repository layout (important gotcha)

The repository root does **not** contain loose source files — almost everything is
packaged inside `.zip` submissions plus two `.docx` narratives:

```
README.md                              CS 499 enhancement narrative (start here)
CLAUDE.md                              this file
CS330 Project One Collins.docx         original design/reflection doc
CS499_Milestone_Two_Narrative_Collins.docx   enhancement narrative (Word)

CS499_Milestone_Two_Daniel_Collins.zip ← THE CURRENT/LATEST STATE. Contains:
    enhanced_scene_Daniel_Collins.html     the Three.js port (the active artifact)
    originals/                             frozen C++ source (MainCode, SceneManager,
                                           ViewManager + headers)

Historical C++ milestone snapshots (oldest → newest), kept for reference only:
    Better_3_2_Assignment_Submission.zip   early single-object scene
    4_3_Milestone_Three_Submission.zip
    5_3_Milestone_Submission.zip
    6_3_Milestone_Submission.zip           most complete C++ scene (SceneManager ~1,850 lines)
    7_1_Final_Project_Submission.zip       CS 330 final (same tree as 6_3)
```

**To find the current source, extract the zip first.** The most recent and canonical
copy of every file lives in `CS499_Milestone_Two_Daniel_Collins.zip`; the newest C++
snapshot is `6_3_Milestone_Submission.zip` / `7_1_Final_Project_Submission.zip`.

```bash
# Work on the live artifact:
unzip -o CS499_Milestone_Two_Daniel_Collins.zip -d /tmp/scene
# Read the latest C++ reference:
unzip -o 6_3_Milestone_Submission.zip '6_3_Milestone_Submission/Source/*' -d /tmp/cpp
```

The zips also contain build artifacts you should ignore and never edit: `*.exe`,
`glew32.dll`, `.vs/`, `*.ipch`, `*.suo`, `*.vcxproj*`, `.sln`. Only the files under
`Source/` and `shaders/`, and the `.html`, are meaningful source.

## Build / run / test

There is **no build system, package manager, test suite, linter, or CI** in this repo.
Do not look for `npm`, `make`, `cmake`, or a test runner — none exist.

- **Enhanced web artifact:** open `enhanced_scene_Daniel_Collins.html` directly in any
  modern browser. No server, no build, no dependencies to install. It pulls Three.js
  r128, OrbitControls, and dat.GUI from CDNs at runtime (so viewing it requires
  internet access).
- **Original C++ app:** builds only on Windows with Visual Studio and the
  OpenGL + GLFW + GLEW + GLM stack (`7-1_FinalProjectMilestones.sln`). It cannot be
  built or run in this Linux environment — treat it as read-only reference.

"Verifying a change" here means loading the HTML in a browser and confirming the scene
renders and the GUI controls work — there are no automated checks.

## Conventions of the enhanced HTML (`enhanced_scene_Daniel_Collins.html`)

Match the existing style when editing. It is deliberately a single ~460-line file with
everything inline (one `<style>` block, one `<script>`), vanilla JS, no modules or
bundler. Keep it self-contained.

- **Section structure** — the script is organized by ALL-CAPS comment banners in this
  order: `SCENE SETUP`, `CONTROLS`, `PROCEDURAL TEXTURES`, `MATERIALS`, `SHARED GEOS`,
  `SCENE OBJECTS`, `LIGHTING`, `GUI PANEL`, `RESPONSIVE RESIZE`, `ANIMATION LOOP`.
  Add new code inside the matching section rather than at the end of the file.
- **Textures are procedural, not files.** `createCanvasTexture(w, h, drawFn)` paints a
  `<canvas>` (marble, gold, copper, opal, stone, whitewash, ground, sky) so the file
  stays self-contained with no external images. Add new textures the same way — do not
  reference external image URLs.
- **Geometry helpers** — `addCylinder`, `addTaperedCylinder`, `addCone`, `addBox`, and
  the generic `addMesh` all take `(mat, sx,sy,sz, rx,ry,rz, px,py,pz)` and internally
  convert degrees→radians and offset Y by half-height. This intentionally mirrors the
  original OpenGL `SetTransformations(scale, Xrot, Yrot, Zrot, position)` semantics
  where objects sit on their base; comments cross-reference the old C++ transforms.
  Reuse these helpers and the shared geometry singletons (`cylinderGeo`, `coneGeo`,
  etc.) instead of constructing meshes ad hoc.
- **Interactivity** — camera uses `OrbitControls` (with damping); lighting is tunable
  live through a `dat.GUI` panel (`Sunlight` and `Environment` folders). New adjustable
  parameters should be wired into the GUI to preserve the "interactive lighting" outcome.
- **Rendering** — shadow mapping (`PCFSoftShadowMap`) and ACES filmic tone mapping are
  on; meshes set `castShadow`/`receiveShadow`. Keep these when adding objects.
- Style: 2-space indentation, `const`/`let`, lowerCamelCase names.

## Conventions of the original C++ (reference only)

If asked to explain or trace behavior back to the source, know that this follows an
SNHU-provided framework (author banner credits *Brian Battersby, SNHU Instructor*):

- `MainCode.cpp` owns GLFW/GLEW init and the render `while` loop; it wires together the
  `ShaderManager`, `SceneManager`, and `ViewManager` globals.
- `SceneManager` is where the student work lives — `PrepareScene()`, `RenderScene()`,
  `LoadSceneTextures()`, `DefineObjectMaterials()`, `SetupSceneLights()`, and a series
  of `RenderFirst()`…`RenderSeventh()` / `Render*Spire()` methods that each draw part of
  the castle. Materials and textures are looked up by string `tag`.
- `ViewManager` handles camera/projection and keyboard+mouse input.
- Shaders: `shaders/vertexShader.glsl`, `shaders/fragmentShader.glsl`.
- Framework files `ShaderManager` and `ShapeMeshes` are referenced by the source but are
  instructor-provided and not all included in every snapshot.

The enhanced HTML deliberately maps onto this structure (its comments say things like
"replaces the while loop in MainCode.cpp"). When porting or reconciling behavior, use
`6_3`/`7_1` as the authoritative C++ version.

## Git workflow

- Active development branch for this work: `claude/claude-md-docs-juydwj`. Develop,
  commit, and push there; create it from the latest `main` if needed. Do not push to
  `main` or another branch without explicit permission.
- History is mostly binary/`.docx`/`.zip` uploads with terse messages
  ("Add files via upload", "Update README.md"). Prefer clear, descriptive commit
  messages going forward.
- Do **not** commit extracted zip contents, build artifacts (`*.exe`, `*.dll`,
  `.vs/`, `*.ipch`), or files unzipped into `/tmp`. There is no `.gitignore`; keep the
  root limited to the curated `.zip`, `.docx`, `.md`, and (if surfaced) `.html`
  deliverables.
- Do not create a pull request unless explicitly asked.

## Working guidance for AI assistants

- For "edit / improve / fix the 3D scene" → the target is
  `enhanced_scene_Daniel_Collins.html` (inside the CS 499 zip). Keep it a single
  self-contained file with no build step.
- For "how did the original work" / "port X from the C++" → read the C++ under the
  `6_3`/`7_1` (or `CS499.../originals/`) zips as reference; that code is not built here.
- Preserve the portfolio framing: changes should keep the deliverable openable by anyone
  in a browser and keep the narrative (`README.md`) accurate to what the code does.
- This is one person's academic ePortfolio (Daniel Collins). Keep author attribution and
  the CS 330 → CS 499 enhancement story intact.
