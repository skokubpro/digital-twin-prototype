
# 🏠 Living Room - Digital Twin  Prototype

A browser-based digital twin of a real living room built using **3D Gaussian Splatting**. The project combines a **photorealistic 3D reconstruction** with **simulated IoT sensors** and a **simple decision engine** that provides real-time recommendations for comfort, energy efficiency, and safety.

---

## 📸 Preview


<img width="1000"  alt="Screenshot 2026-06-26 023223" src="https://github.com/user-attachments/assets/4ca6b377-7d61-4090-80b0-71681729e0e7" />

---

## 🧩 Project Components


The prototype consists of three main components and is built as a **single HTML application** using only HTML, CSS, and vanilla JavaScript. No frameworks, backend, or build tools are required.

---

## Component 1 — Photorealistic 3D Reconstruction

The living room was reconstructed in two stages.

### First Attempt

The room was photographed using a **Samsung Galaxy S26 Ultra** in Pro Mode (ISO 400, 1/60s shutter speed, white balance locked at 4600K, manual focus set at 0.76). A structured perimeter walking path was followed at three different heights: eye level, chest level, and waist level. The best **106 frames** were selected using **Sharp Frames by Reflct** (Best-N selection, sharpness scoring) and uploaded to **Polycam** for Gaussian Splat processing. Although the reconstruction was successful, it contained many problems such as missing areas, floaters near the windows caused by light bleeding through the blinds, and poor coverage in the corners. Because of these issues, a second reconstruction method was used.

### Second Attempt

The room was scanned using **Scaniverse**. Instead of scanning the entire room at once, it was divided into seven smaller sections:

- Sofa area 1
- Sofa area 2
- Dining corner
- TV wall and Windows
- Bookshelf
- Freezer area
- Room centre

Each section was processed separately, producing a dense point cloud. The scans were merged and exported as a single PLY file.

The PLY file was uploaded into **Splat Editor**, where floaters were manually removed. The cleaned file was then exported as a `.splat` file, zipped, and uploaded to GitHub.

The browser loads the `.splat` file using the `gaussian-splats-3d` library and renders it in real time with **Three.js**. WASD + arrow key movement controls were implemented in vanilla JavaScript, combined with the library's built-in mouse orbit behaviour. Users can freely explore the room using keyboard and mouse controls.

---

## Component 2 — Live Data Overlays

Five virtual IoT sensors are displayed directly inside the 3D scene. The sensor values are generated in **JavaScript** using Gaussian noise (Box-Muller transform), which adds realistic random variations. Some sensors also follow daily patterns, such as changes in temperature and occupancy throughout the day.

Each sensor is placed at its actual location inside the room. As the camera moves, the sensor labels stay fixed to the correct objects by projecting their 3D coordinates onto the screen every animation frame using the Three.js `Vector3.project()` method.

| Sensor | Location | Simulation |
|---|---|---|
| 🌡️ Temperature | Near the radiator and window | Daily temperature cycle with small random changes (±0.4°C). |
| 👥 Occupancy | Centre of the room | Number of people changes throughout the day, with higher activity in the morning and evening. |
| 🔌 TV Device State | Near the TV | TV turns on or off based on simulated occupancy. |
| 🧊 Freezer Temperature | Near the freezer | Temperature cycles normally between −24°C and −18°C. |
| 💨 CO₂ Air Quality | Ceiling sensor | CO₂ increases as occupancy increases, with small random variations (±18 ppm). |

---

## Component 3 — Decision Layer

The system checks all sensor values every 3 seconds and generates recommendations to improve comfort, safety, and energy efficiency based on simple rules. The decision engine is entirely written in **vanilla JavaScript** and uses easy-to-understand rule-based logic, making every recommendation transparent and easy to explain.

The confidence values represent how certain the system is that the recommendation is appropriate. They are predefined values for this prototype rather than values calculated by machine learning. In the future, machine learning could be added to detect unusual behaviour automatically.

| Rule | Trigger | Recommendation | Confidence |
|---|---|---|---|
| Thermal Comfort | Temperature > 26°C and the room is occupied | Turn on ventilation or lower the thermostat. | 94% |
| Energy Saving | TV is ON and occupancy = 0 | Turn off the TV automatically to save energy. | 97% |
| Freezer Safety | Freezer temperature > −18°C or freezer door is open | Check the door seal or close the freezer door. | 96–99% |
| Air Quality | CO₂ level > 1000 ppm | Open a window to improve ventilation. | 91% |

---

## 🛠️ Technology Stack

| Layer | Technology |
|---|---|
| Room capture & processing | Scaniverse (7 scans) |
| Floater removal | Splat Editor (manual) |
| 3D Rendering | gaussian-splats-3d |
| Graphics Engine | Three.js |
| Sensor Simulation | Vanilla JavaScript + Box-Muller Gaussian noise |
| Decision Layer | Rule-based logic |
| Frontend | HTML, CSS, Vanilla JavaScript |

---

## 🏗️ Processing Pipeline

```
Real Living Room (Lahti, Finland)
        │
        ▼
Scaniverse
(7 separate scans)
        │
        ▼
Merged into one PLY model
        │
        ▼
Splat Editor
(Remove floaters manually)
        │
        ▼
Export as .splat
        │
        ▼
Browser Viewer
        │
        ├── Photorealistic 3D scene
        ├── Live sensor overlays
        └── Decision recommendations
```

---

## 🚀 Running the Project

### GitHub Pages

1. Fork or clone this repo
2. Go to **Settings → Pages → Deploy from main branch**
3. Open the Pages URL in **Chrome**
4. The `scene.zip` file is loaded automatically
5. The 3D scene loads automatically

> **Note on the splat file:** The repository contains two versions of the splat. `3d_livingroom.splat` is the original exported file, and `scene.zip` is an identical copy renamed with a `.zip` extension. This workaround is necessary because GitHub Pages applies gzip compression when serving `.splat` files, which causes the viewer library to fail. Renaming the file to `.zip` bypasses this compression and loads correctly.

---

## 🎮 Controls

| Key | Action |
|---|---|
| `W` | Move forward |
| `S` | Move backward |
| `A` | Move left |
| `D` | Move right |
| `↑` / `↓` | Move up or down |
| `Shift` + any above | Fast mode (3.5× speed) |
| Mouse Drag | Orbit / Rotate camera |
| Mouse Wheel | Zoom in / out |

---

## 📁 Repository Structure

```
digital-twin-prototype/
│
├── index.html              ← Complete viewer (3D + sensors + decisions)
├── 3d_livingroom.splat     ← Original Gaussian Splat file
├── scene.zip               ← Copy of the same .splat file renamed with a .zip extension.
│                             GitHub Pages automatically applies gzip compression to
│                             .splat files, which prevents them from loading correctly.
│                             Using the renamed scene.zip file avoids this issue while
│                             still loading the same Gaussian Splat data.
├── README.md               ← Project documentation
└── docs/
    └── From Physical Space to Service Platform.pdf         ← 1-page research write-up
```

---

## ⚠️ Current Limitations

| Limitation | Reason | Possible Improvement |
|---|---|---|
| Partial ceiling reconstruction | The mobile scan captured walls and furniture better than the ceiling. | Scan the ceiling from more angles to improve coverage. |
| Small seams between scan sections | The room was scanned in seven separate parts and then merged together. | More careful alignment or additional cleanup could reduce these seams. |
| Minor window and glass artefacts | Glass and transparent surfaces are difficult to reconstruct accurately. | Closing the blinds helped, but a LiDAR scanner or professional 3D scanner could further improve the result. |
| Simulated sensor data | No real IoT sensors were connected to the system. | Replace the simulated data with live readings from physical sensors in the future. |
| Rule-based decision engine | The system uses predefined rules instead of learning from data. | Machine learning could be added in future versions to detect unusual patterns and improve recommendations automatically. |

---

## Summary

- **3D Reconstruction:** Scaniverse + Splat Editor
- **3D Viewer:** gaussian-splats-3d + Three.js
- **Sensor Simulation:** Vanilla JavaScript
- **Decision Layer:** Rule-based recommendations


