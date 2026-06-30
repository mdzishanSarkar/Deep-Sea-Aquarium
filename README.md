<div align="center">

# 🌊 Deep Sea Aquarium

### An Interactive 2D Ocean Simulation built with OpenGL & GLUT

[![Language](https://img.shields.io/badge/Language-C%2B%2B-blue?style=for-the-badge&logo=cplusplus)](https://isocpp.org/)
[![Graphics](https://img.shields.io/badge/Graphics-OpenGL%20%2B%20GLUT-green?style=for-the-badge&logo=opengl)](https://www.opengl.org/)
[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20macOS-lightgrey?style=for-the-badge)]()
[![Status](https://img.shields.io/badge/Status-Complete-success?style=for-the-badge)]()

<br>

> *Dive into a living, breathing underwater world — complete with swimming fish, a bobbing octopus, gliding turtle, animated coral, day/night cycles, and interactive feeding.*

<br>

</div>

---

## 📸 Preview

| Day Mode 🌞 | Night Mode 🌙 |
|:-----------:|:-------------:|
| Sun rays pierce the water surface, casting light across the sandy seabed | Stars fill the sky, the moon glows with a stencil-masked crescent shadow |

> **Resolution:** 1301 × 764 px &nbsp;|&nbsp; **Target FPS:** ~60 (16 ms timer loop)

---

## ✨ Features at a Glance

| Category | What's Inside |
|---|---|
| 🐟 **Fish System** | Up to **100 fish**, 5 unique species (Koi, Tang, Angelfish, Clownfish, Cobalt), autonomous AI-steering with smooth turning |
| 🐙 **Octopus** | Full body animation — swaying, bobbing, and tilting in real time |
| 🐢 **Sea Turtle** | Glides gracefully across the scene with independent update logic |
| 🌊 **Water** | Three layered sine-wave surfaces with alpha blending for realistic depth |
| ☀️ **Day Mode** | Sun with Midpoint Circle fill, 17 soft volumetric ray shafts |
| 🌙 **Night Mode** | Stencil-buffered crescent moon, 35 random stars, dark blue overlay tint |
| 🪸 **Seabed** | Hand-crafted sandy floor, dark/light stones, seashell, starfish, coral, seagrass |
| 🫧 **Bubbles** | Togglable rising bubbles with fade-out alpha |
| 🍖 **Food Drop** | Click anywhere or press `F` to drop food — fish home in on it |
| ⏸️ **Pause** | Freeze the entire simulation with one key |

---

## 🗂️ Project Structure
```
Deep Sea Aquarium/
│
├── .vscode/
│   └── tasks.json                  # VS Code build task configuration
│
├── include/                        # All header/library includes
│   ├── GL/
│   │   ├── freeglut_ext.h          # FreeGLUT extension functions
│   │   ├── freeglut_std.h          # FreeGLUT standard definitions
│   │   ├── freeglut.h              # Main FreeGLUT header
│   │   └── glut.h                  # Classic GLUT compatibility header
│   ├── glad/
│   │   └── glad.h                  # OpenGL function loader (GLAD)
│   ├── GLFW/
│   │   ├── glfw3.h                 # GLFW core API header
│   │   └── glfw3native.h          # GLFW native platform access
│   └── KHR/
│       └── khrplatform.h           # Khronos platform type definitions
│
├── lib/                            # Pre-compiled static libraries
│   ├── libfreeglut.a               # FreeGLUT static library
│   └── libglfw3.a                  # GLFW3 static library
│
├── src/                            # All source code
│   ├── Lab/                        # Lab exercise files
│   ├── Project/                    # Project-specific files
│   ├── glad.c                      # GLAD OpenGL loader source
│   └── main.cpp                    # 🎯 Main simulation — all rendering & logic
│
├── freeglut.dll                    # FreeGLUT runtime DLL (Windows)
└── myProgram.exe                   # Compiled executable output
```

> This is a single-file project. All drawing routines, animation state, and GLUT callbacks live in `main.cpp`.

---

## 🧱 Architecture Overview

```
main()
 ├── glutInit / Window Setup
 ├── initFish()           → Spawns fish with random species, position, velocity
 ├── initBubbles()        → Creates bubble pool
 └── GLUT Loop
      ├── display()        → Master render call (ordered back-to-front)
      │    ├── water()              ← Animated 3-layer wave system
      │    ├── drawSun/Moon()       ← Day or night celestial body
      │    ├── drawSand*()          ← Layered sandy seabed polygons
      │    ├── drawStones/Shells()  ← Decorative seabed elements
      │    ├── drawOctopus()        ← Animated with push/pop matrix
      │    ├── drawTurtle()         ← Independent animation
      │    ├── drawCoral/Leaf()     ← Static scenery
      │    ├── drawFish()           ← All active fish, per-species geometry
      │    ├── drawBubbles()        ← Rising alpha-fading spheres
      │    └── drawNightOverlay()   ← Moon + stars (night mode only)
      │
      ├── timer()          → Updates physics (fish, bubbles, octopus) at 16 ms
      ├── keyboard()       → All key bindings
      └── mouse()          → Left-click to drop food
```

---

## 🎮 Controls

| Input | Action |
|:-----:|--------|
| `D` | Switch to **Day** mode ☀️ |
| `N` | Switch to **Night** mode 🌙 |
| `B` | Release a burst of **bubbles** 🫧 |
| `F` | Drop **food** at a random position 🍖 |
| **Left Click** | Drop food exactly where you click 🖱️ |
| `1` – `5` | Select active fish **species** |
| `+` / `=` | Add a fish (up to 100) |
| `-` / `_` | Remove a fish (minimum 1) |
| `R` | **Reset** all fish positions |
| `Space` | **Pause / Resume** simulation |
| `Esc` | **Quit** |

---

## 🐠 Fish Species

Each species has its own body shape, color palette, pattern, and tail geometry:

| # | Species | Body Color | Accent | Distinguishing Feature |
|---|---------|-----------|--------|----------------------|
| `1` | **Koi** | Orange | Gold | Elongated body, flowing double tail |
| `2` | **Blue Tang** | Deep Blue | Yellow | Oval body, horizontal stripe details |
| `3` | **Angelfish** | Violet-Purple | White | Tall body, vertical white stripes |
| `4` | **Clownfish** | Coral Pink | White | Rounded body, classic 3-bar banding |
| `5` | **Cobalt** | Cobalt Blue | Silver | Streamlined, dark banding + spot |

---

## 🔧 Core Rendering Techniques

This project demonstrates a wide range of classic OpenGL 2D techniques:

- **`GL_POLYGON` / `GL_QUAD_STRIP`** — filled sand terrain, fish bodies, sun, moon
- **`GL_LINE_LOOP` / `GL_LINE_STRIP`** — smooth anti-aliased outlines (`GL_LINE_SMOOTH`)
- **`GL_POINTS`** — stone scatter using `GL_POINT_SMOOTH` for soft pebble look
- **GLU Tessellator** — handles concave polygons that `GL_POLYGON` can't fill correctly
- **`GL_BLEND`** — alpha transparency on water layers, bubbles, night overlay, sun rays
- **`GL_STENCIL_TEST`** — crescent moon shadow carved out via stencil masking
- **Midpoint Circle Algorithm** — hand-rolled filled circle drawing for the Sun
- **Matrix Stack (`glPushMatrix` / `glPopMatrix`)** — octopus pivot-point animation (sway, bob, tilt)
- **Sine-wave Math** — three-frequency composited wave surfaces for the water

---

## 🚀 Getting Started

### Prerequisites

You need a C++ compiler and the GLUT (or FreeGLUT) library installed.

**Ubuntu / Debian:**
```bash
sudo apt-get update
sudo apt-get install freeglut3-dev build-essential
```

**macOS (Homebrew):**
```bash
brew install freeglut
```

**Windows:**
- Install [freeglut for Windows](http://freeglut.sourceforge.net/) and link it in your IDE (Visual Studio / Code::Blocks / MinGW).

---

### Compile & Run

**Linux / macOS:**
```bash
g++ main.cpp -o aquarium -lGL -lGLU -lglut -lm
./aquarium
```

**Windows (MinGW):**
```bash
g++ main.cpp -o aquarium.exe -lfreeglut -lopengl32 -lglu32 -lm
aquarium.exe
```

> ✅ No external dependencies beyond OpenGL, GLU, and GLUT. Works with both legacy GLUT and FreeGLUT.

---

## 🎨 Scene Elements

<details>
<summary><strong>🌊 Water System</strong> (click to expand)</summary>

The water is rendered as **three independent sine-wave layers**, each with different frequency, amplitude, phase offset, and alpha transparency. Together they create a convincing illusion of surface depth and motion.

```
Layer 0 — Primary surface  (amplitude: 14px, semi-transparent teal crest)
Layer 1 — Mid-depth wave   (amplitude: 11px, offset phase)
Layer 2 — Deep shimmer     (amplitude:  9px, subtle dark blue)
```

Each layer is updated every frame using the global `gTime` counter.

</details>

<details>
<summary><strong>☀️ / 🌙 Day & Night Cycle</strong> (click to expand)</summary>

**Day Mode:**
- Sun rendered at top-right with a Midpoint Circle fill in two passes (glow + core)
- 17 volumetric ray shafts drawn as tapered `GL_QUADS` with alpha falloff

**Night Mode:**
- Full-screen dark blue overlay with 62% alpha
- 35 randomly scattered stars rendered as `GL_POINTS`
- Crescent moon using **stencil buffer**: a white circle is drawn first, then a shadow circle is masked into it — creating a crescent without any math tricks

</details>

<details>
<summary><strong>🐟 Fish AI</strong> (click to expand)</summary>

Each fish has:
- `x, y` — position
- `vx, vy` — velocity (direction + speed)
- `size` — body scale
- `species` — controls geometry and color
- `phase` — drives fin/tail oscillation
- `dir` — facing direction (for sprite flip)

Fish steer toward food when `foodActive == 1`, otherwise they swim autonomously and bounce off screen edges. A smooth angle interpolation prevents jarring direction changes.

</details>

<details>
<summary><strong>🪸 Seabed Scene</strong> (click to expand)</summary>

The seabed is built from multiple hand-crafted polygon layers:

| Element | Technique |
|---------|-----------|
| Main sandy floor | `drawSandFill()` — `GL_QUAD_STRIP` from curve to bottom |
| Sand highlights | `drawTopMiddleSand()`, `drawBottomLeft/RightSand()` |
| Dark edge shading | `drawEdgeStrip()` for depth illusion |
| Stones | `drawPointCloud()` — scattered points in two tones |
| Seashell & Starfish | Custom polygon geometry |
| Coral & Seagrass | Multi-branch polygon shapes with leaf bone rendering |

</details>

---

## 📐 Constants & Limits

| Constant | Value | Description |
|----------|-------|-------------|
| `MAX_FISH` | `100` | Maximum simultaneous fish |
| `MAX_BUBBLES` | `60` | Maximum simultaneous bubbles |
| Default fish count | `15` | Fish alive at startup |
| Window size | `1301 × 764` | Initial resolution (resizable) |
| Timer interval | `16 ms` | ~62.5 FPS target |
| `gTime` increment | `0.022` per frame | Global animation clock |

---

## 📚 Concepts Demonstrated

This project is an excellent reference for learning:

- [x] OpenGL immediate-mode 2D rendering pipeline
- [x] Alpha blending and transparency effects
- [x] Stencil buffer usage for masking
- [x] GLU Tessellator for concave polygon fill
- [x] Midpoint Circle Algorithm (rasterisation from scratch)
- [x] Sine-wave based procedural animation
- [x] Matrix transformations for scene-graph-style animation
- [x] Simple autonomous agent steering (fish AI)
- [x] Day/Night scene state management
- [x] Resizable viewport with orthographic projection

---
