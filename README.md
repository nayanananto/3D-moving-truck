# 🚛 A Truck — Three.js Computer Graphics Project

**Course:** Computer Graphics Lab (CSE 4204)
**Institution:** Ahsanullah University of Science & Technology, Dept. of Computer Science & Engineering

| Member | Student ID |
|---|---|
| Ananto Nayan Bala | 20210204028 |
| Tasmia Hossain | 20210204038 |

---

## 📋 Overview

A fully interactive 3D heavy freight truck rendered in real-time using Three.js and custom GLSL shaders. The scene features a drivable truck on a procedurally generated curved road, complete with physically consistent wheel rotation, a dynamic orbiting light, and a smoke particle system — all implemented in a single self-contained HTML file with no build step required.

---

## ✨ Features

### 3D Scene
- Truck body (cabin + cargo) built from `BoxGeometry`, `CylinderGeometry`, and `PlaneGeometry` primitives, grouped under a single `THREE.Group`
- Procedurally generated canvas textures for the truck body (ROADLINE LOGISTICS livery), wheels (tread + spokes + hub), and road (aged asphalt with lane markings) — no external image files
- Scene fog (`near: 18, far: 60`) and dark blue-grey background for depth and atmosphere

### Custom Shaders
Both truck body and wheels use `THREE.ShaderMaterial` with custom GLSL:

- **Shared Vertex Shader** — transforms vertices to clip space and passes UV, view-space position, and view-space normal to fragment shaders
- **Body Fragment Shader** — Blinn-Phong diffuse + specular (shininess 36) + rim highlight for a metallic silhouette glow
- **Wheel Fragment Shader** — sin-based procedural tread modulation on the U coordinate + edge glow tinted blue-white

### Lighting
| Light | Purpose |
|---|---|
| `HemisphereLight` | Ambient sky/ground gradient |
| `DirectionalLight` | Fill and shadow light |
| `PointLight` | Orbiting inspection light with `PCFSoftShadowMap` |

The `PointLight` orbits the truck at radius 9 units, with a visible golden light orb mesh and a translucent teal torus ring marking its path. Its world-space position is transformed to view space each frame before being passed to the `uLightPosView` shader uniform.

### Camera
Spherical coordinate system (`theta`, `phi`, `radius`) updated every frame. Supports auto-orbit mode, mouse drag to rotate, and scroll wheel zoom (range: 6–24 units). Always targets a point 1.9 units above the truck center.

---

## 🎮 Controls

### Keyboard
| Key | Action |
|---|---|
| `W` / `↑` | Drive forward |
| `S` / `↓` | Drive backward / reverse |
| `A` / `←` | Steer left |
| `D` / `→` | Steer right |
| `Space` | Brake (`speed *= 0.88` per frame) |
| `C` | Toggle auto camera orbit |
| `L` | Toggle orbiting inspection light |
| `H` (hold) | Emit horn smoke particles |
| `R` | Reset scene to initial state |

### Mouse
| Input | Action |
|---|---|
| Click + Drag | Orbit camera around truck |
| Scroll Wheel | Zoom in/out (6–24 units) |
| Horn Smoke Button | Trigger smoke particle system |

---

## ⚙️ Technical Implementation

### Truck Construction
All parts are grouped under a single `THREE.Group`. Front wheels are placed inside `steeringPivot` groups whose local Y-rotation equals the steer angle, producing realistic Ackermann-like steering visuals.

Wheel spin is computed from truck speed and wheel radius for physically consistent rolling:
```js
wheelSpin = state.speed * dt / wheelRadius;
spinNode.rotation.z -= wheelSpin;
```

### Road Generation
A `CatmullRomCurve3` spline with custom `BufferGeometry` — points are sampled along the curve, tangent and lateral vectors computed, and vertex pairs placed to form a ribbon mesh, producing a naturally curved road.

### Shader Uniform Sync
The `PointLight` position is converted from world space to view space each frame before being passed as a uniform:
```js
tempLightPos.copy(pointLight.position).applyMatrix4(camera.matrixWorldInverse);
```

### Smoke Particles
A particle system emits from the front exhaust pipe tip when `H` is held or the Horn Smoke button is pressed. Particles rise with slight lateral drift.

---

## 🛠️ Tech Stack

| Technology | Role |
|---|---|
| HTML5 / CSS3 | Page structure and HUD styling |
| JavaScript (ES Modules) | Scene logic and animation loop |
| Three.js r161 | 3D rendering (loaded via importmap) |
| WebGL | GPU rendering backend |
| GLSL | Custom vertex and fragment shaders |
| HTML5 Canvas API | Procedural texture generation at runtime |
| VS Code + Live Server | Development environment |
| Node.js | Prerequisite for Three.js local module setup |

---

## 🚀 Running the Project

1. Make sure **Node.js** is installed
2. Open the project folder in **VS Code**
3. Start **Live Server** (right-click `index.html` → Open with Live Server)
4. Open `http://localhost:5500` in your browser

> The project is fully self-contained in a single HTML file. No npm install or build step needed.

---

## 👥 Contributions

| Member | Contributions |
|---|---|
| **Ananto Nayan Bala** | 3D truck modeling, custom GLSL shaders, procedural texture generation, lighting setup, camera system, wheel rotation and movement animation |
| **Tasmia Hossain** | Keyboard vehicle controls, mouse camera interaction, smoke particle system, scene environment design (fog, background, layout) |
