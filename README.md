
# 🏠 Living Room - Digital Twin  Prototype

A photorealistic, browser-based **digital twin** of a real residential living room, built using **3D Gaussian Splatting** for the spatial reconstruction and a simulated **IoT sensor layer** for real-time environmental monitoring and automated decision support.

---

## 📸 Preview


<img width="1000"  alt="Screenshot 2026-06-26 023223" src="https://github.com/user-attachments/assets/4ca6b377-7d61-4090-80b0-71681729e0e7" />

---

## 🧩 Project Components

This prototype fulfils all three required components of the task:

### Component 1 — Photorealistic 3D Reconstruction

The room was captured using **Scaniverse** on a mobile device. Because Scaniverse works best on contained areas rather than large open spaces, the room was divided into **7 separate scan segments** — each covering a distinct portion such as the sofa area, dining corner, TV wall, bookshelf, freezer area, door wall, and room centre. Scaniverse processes each scan on-device using photogrammetry, generating a dense point cloud per segment. The 7 scans were then merged and exported as a unified **PLY file**.

The PLY file was imported into a **Splat Editor** where floaters — isolated Gaussian splats that appear outside the room boundary or in mid-air due to scan overlap artefacts — were manually removed by lasso-selecting and deleting them. The cleaned file was exported as a `.splat` file, zipped, and uploaded to GitHub.

The resulting file is rendered in the browser using `@mkkellogg/gaussian-splats-3d` and is fully navigable with WASD keyboard + mouse orbit controls.

### Component 2 — Five Live Data Overlays


This component simulates five IoT sensors that display live data inside the 3D room. The sensor values are generated using Gaussian noise (Box-Muller transform), which adds realistic random variations. Some sensors also follow daily patterns, such as changes in temperature and occupancy throughout the day.

Each sensor is placed at its actual location in the 3D Gaussian splat model and remains attached to that position as the user moves around the scene.

| Sensor | Location | Simulation |
|---|---|---|
| 🌡️ Temperature | Near the radiator and window wall | Simulates a daily temperature cycle with small random fluctuations (±0.4°C). |
| 👥 Occupancy | Centre of the room | Simulates the number of people in the room, with higher occupancy during morning and evening hours. |
| 🔌 TV Device State | Near the TV monitor | Turns the TV on or off based on the simulated room occupancy. |
| 🧊 Freezer Temperature | Near the freezer | Simulates normal compressor cycles between −24°C and −18°C. |
| 💨 CO₂ Air Quality | Ceiling sensor | CO₂ level increases as occupancy increases, with small random variations (±18 ppm). |

---

### Component 3 — Decision Layer

The decision layer checks the sensor readings every 3 seconds and generates recommendations to improve comfort, safety, and energy efficiency. The system uses simple rule-based logic, making every decision easy to understand and explain. In the future, machine learning could be added to detect unusual behaviour automatically.

| Rule | Trigger Condition | Recommendation | Confidence |
|---|---|---|---|
| Thermal Comfort | Temperature > 26°C and the room is occupied | Turn on ventilation or lower the thermostat. | 94% |
| Energy Saving | TV is ON and occupancy = 0 | Turn off the TV automatically to save energy. | 97% |
| Freezer Safety | Freezer temperature > −18°C or freezer door is open | Check the door seal or close the freezer door. | 96–99% |
| Air Quality | CO₂ level > 1000 ppm | Open a window to improve ventilation. | 91% |

---


