# 💎 Text-to-3D AI Jewellery Visualiser

> An NLP-powered generative 3D jewellery design platform — type a description, get an interactive 3D preview in real time.

![Python](https://img.shields.io/badge/NLP-Pipeline-blue?style=flat-square)
![Three.js](https://img.shields.io/badge/Three.js-r128-black?style=flat-square)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-yellow?style=flat-square)
![HTML5](https://img.shields.io/badge/HTML5-Canvas-orange?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

---

## 🎯 Project Overview

This project demonstrates an end-to-end **NLP → 3D Generation → Rendering pipeline** applied to the jewellery domain. A user types a natural language description (e.g. *"a delicate rose gold ring with a teardrop sapphire"*), and the system:

1. **Parses intent** using a keyword-based NLP pipeline
2. **Extracts structured parameters** (type, metal, gemstone, style)
3. **Procedurally generates** a 3D model in real time
4. **Renders it interactively** with physically-based rendering (PBR) materials

This project was built to demonstrate core competencies required for AI-driven digital jewellery platforms, including NLP pipelines, generative 3D systems, and translating AI outputs into commercially usable interfaces.

---

## 🚀 Live Demo

**No installation required.** Simply open `index.html` in any modern browser.

```bash
git clone https://github.com/YOUR_USERNAME/jewellery-3d-ai.git
cd jewellery-3d-ai
open index.html   # macOS
# or double-click index.html on Windows/Linux
```

> Requires an internet connection on first load to fetch Three.js from CDN. See [Offline Setup](#offline-setup) for local-only usage.

---

## ✨ Features

| Feature | Description |
|---|---|
| **NLP Intent Parsing** | Extracts jewellery type, metal, gemstone, and style from free-text input |
| **Real-time 3D Generation** | Procedural geometry generation with no pre-built models |
| **PBR Materials** | Physically-based rendering for gold, silver, rose gold, platinum |
| **Gemstone Rendering** | Light transmission & refraction simulation (diamond, sapphire, emerald, ruby, rose quartz) |
| **Interactive Viewer** | Drag to rotate, scroll to zoom, right-drag to pan |
| **5 Jewellery Types** | Ring, Necklace, Earring, Bangle, Pendant |
| **5 Style Variations** | Classic, Modern, Vintage, Geometric, Minimalist |
| **Metadata Extraction** | Estimated weight, complexity score, NLP keyword tags |
| **Zero Dependencies** | Runs entirely in the browser, no backend required |

---

## 🏗️ System Architecture

```
User Input (Natural Language)
          │
          ▼
┌─────────────────────────┐
│     NLP Parse Engine     │   keyword extraction, intent detection,
│  (nlpParse function)     │   entity mapping → structured parameters
└─────────────────────────┘
          │
          ▼
┌─────────────────────────┐
│  Parameter Resolution    │   type + metal + gemstone + style
│  & Validation           │   resolved to geometry/material configs
└─────────────────────────┘
          │
          ▼
┌─────────────────────────┐
│  3D Geometry Builder     │   buildRing / buildNecklace / buildEarring
│  (Procedural Generation) │   buildBangle / buildPendant
└─────────────────────────┘
          │
          ▼
┌─────────────────────────┐
│  PBR Material Engine     │   MeshStandardMaterial (metals)
│                          │   MeshPhysicalMaterial (gems)
└─────────────────────────┘
          │
          ▼
┌─────────────────────────┐
│  Three.js Renderer       │   WebGL rendering, lighting rig,
│  (WebGL)                 │   ACES filmic tone mapping
└─────────────────────────┘
          │
          ▼
    Interactive 3D Viewer
    + Metadata Card Output
```

---

## 🧠 NLP Pipeline — How It Works

The NLP engine (`nlpParse()`) uses a **rule-based entity extraction** approach:

```javascript
// Example: "a delicate rose gold ring with a teardrop sapphire"
//
// Step 1: Lowercase + tokenise input
// Step 2: Match against entity dictionaries
//
//   TYPE     → ring     (matched: "ring")
//   METAL    → rose     (matched: "rose gold")
//   GEMSTONE → sapphire (matched: "sapphire")
//   STYLE    → classic  (default — no style keyword detected)
//   EXTRAS   → ["delicate"] (modifier keywords)
//
// Step 3: Map entities → geometry parameters
// Step 4: Generate 3D model
```

**Entity dictionaries** (in `index.html` → `nlpParse` function):

| Entity | Keywords |
|---|---|
| Ring | ring, band, solitaire |
| Necklace | necklace, chain, collar |
| Earring | earring, drop, stud, hoop |
| Bangle | bangle, bracelet, cuff |
| Pendant | pendant, charm |
| Gold | gold, golden |
| Rose Gold | rose gold, rose |
| Silver | silver, sterling |
| Platinum | platinum, white gold |
| Diamond | diamond |
| Sapphire | sapphire, blue |
| Emerald | emerald, green |
| Ruby | ruby, red |
| Geometric | geometric, angular, hexagonal |
| Vintage | vintage, antique, art deco |
| Minimalist | minimal, simple, dainty |

---

## 🔧 3D Generation — How It Works

Each jewellery type is **procedurally generated** using Three.js geometry primitives. No pre-built 3D models are loaded — everything is computed at runtime.

### Ring Generation
```javascript
// Core band: TorusGeometry
const band = new THREE.Mesh(
  new THREE.TorusGeometry(0.5, tubeRadius, 32, 128),
  metalMaterial
);

// Gemstone setting: prongs (CylinderGeometry) + OctahedronGeometry gem
const gem = new THREE.Mesh(
  new THREE.OctahedronGeometry(0.13, 1),  // faceted gem shape
  gemMaterial  // MeshPhysicalMaterial with transmission
);
```

### Physically-Based Materials
```javascript
// Metal (MeshStandardMaterial)
{ color: 0xc9a84c, roughness: 0.25, metalness: 0.95 }  // Yellow Gold

// Gemstone (MeshPhysicalMaterial — enables light transmission)
{ color: 0x4a7ab5, transparent: true, opacity: 0.85,
  transmission: 0.5, thickness: 0.4 }  // Sapphire
```

### Lighting Rig
The scene uses a 4-light studio rig to simulate jewellery photography:
- **Key light** — warm directional (simulates studio spot)
- **Fill light** — cool directional (reduces harsh shadows)
- **Rim light** — backlight for edge definition
- **Point light** — close warm light for gem sparkle

---

## 📁 Project Structure

```
jewellery-3d-ai/
│
├── index.html          # Main application (self-contained)
├── README.md           # This file
├── LICENSE             # MIT License
│
├── src/
│   ├── nlp.md          # NLP pipeline documentation
│   ├── geometry.md     # 3D geometry builder documentation
│   └── materials.md    # PBR materials documentation
│
└── docs/
    ├── architecture.md  # Full system architecture
    ├── screenshots/     # Add your own screenshots here
    └── CONTRIBUTING.md  # Contribution guidelines
```

---

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| **Three.js r128** | 3D rendering engine (WebGL) |
| **Vanilla JavaScript (ES6+)** | NLP pipeline, geometry generation, application logic |
| **HTML5 / CSS3** | UI layout and styling |
| **WebGL** | GPU-accelerated 3D rendering |
| **Google Fonts** | Cormorant Garamond + Jost typography |

**No frameworks. No build tools. No npm. Just open and run.**

---

## 📐 Offline Setup

To run completely offline, download Three.js locally:

```bash
# Download Three.js r128
curl -o three.min.js https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js
```

Then edit `index.html` line with the Three.js CDN import:
```html
<!-- Replace this: -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

<!-- With this: -->
<script src="three.min.js"></script>
```

---

## 🔮 Future Improvements

- [ ] Integrate a real LLM API (e.g. GPT-4, Claude) for richer NLP understanding
- [ ] Add Shap-E or Point-E for true AI-generated 3D geometry
- [ ] Export designs as `.glb` / `.gltf` files for CAD/CAM workflows
- [ ] Add Speech-to-Text input (Web Speech API or Whisper)
- [ ] Size customisation (ring size, necklace length)
- [ ] Save/load designs with localStorage
- [ ] Multi-angle automated renders for e-commerce
- [ ] LangChain agent integration for multi-turn design conversations
- [ ] Backend API (FastAPI + Python) for server-side NLP processing

---

## 👤 Author

**Girish Kundal**
MSc Artificial Intelligence (Distinction) — Birmingham City University

- 📧 kundalgirish5@gmail.com
- 🔗 [LinkedIn](https://www.linkedin.com/in/girish-kundal/)
- 🐙 [GitHub](https://github.com/girishkundal)

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.

---

## 🙏 Acknowledgements

- [Three.js](https://threejs.org/) — WebGL 3D library
- [Google Fonts](https://fonts.google.com/) — Cormorant Garamond & Jost
- Inspired by the challenge of building AI-powered bespoke jewellery platforms
