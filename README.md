
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

