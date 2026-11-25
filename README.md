# Inside a Fitness Watch — Biometric Telemetry & Hardware Anatomy

> **GVH Hackathon Project**  
> A high-impact, minimalist website explaining how smartwatches measure steps (MEMS Accelerometer) and heart rate (Optical Photoplethysmography). Designed to be understood in under 2 minutes.

---

## 🌐 Live Website

- **Production (Vercel):** [https://fitnesswatch.vercel.app](https://fitnesswatch.vercel.app)
- **Local Server:** `http://localhost:8000` (run with `python3 -m http.server 8000`)

---

## 💡 What's New (Post-Testing Redesign)

1. **Light Minimalist Theme (Apple/Swiss Design)**:
   - Replaced dark backgrounds with clean high-key white (`#ffffff` / `#f5f5f7`), dark metallic hardware models, and crisp typography (`#1d1d1f`).
2. **Real 3D Fitness Watch (`watch.glb`)**:
   - Loaded authentic 3D smartwatch model with OrbitControls (360° drag rotation) and interactive disassemble/explode slider.
3. **Unified All-In-One Accelerometer Studio**:
   - Removed fragmented sequential scenes. The entire mechanism is visible on one screen:
     - Real-time physical silicon MEMS chip simulation (proof mass $m$, suspension springs $k$, fixed/moving capacitor comb plates).
     - Live motion modes: **Walking (1.8 Hz)**, **Running (3.0 Hz)**, **Hand Gesture (Noise)**, **Idle**.
     - "Disassemble MEMS" toggle to inspect micro-components.
     - Live 3-axis graph, magnitude curve, threshold line, and real-time step register.
4. **Unified All-In-One Optical PPG Studio**:
   - Continuous physical cross-section: watch backplate, dual green LEDs ($\lambda = 530\text{ nm}$), photodiode, epidermis, dermis, and pulsating artery.
   - **Crucial Reflected Light Visual**:
     - *Systole (High blood volume)* $\to$ Hemoglobin absorbs light $\to$ **reflected green beam is weak and faint**.
     - *Diastole (Low blood volume)* $\to$ Less absorption $\to$ **reflected green beam is strong and bright**.
     - Photodiode converts fluctuating intensity into electrical pulses.
   - Real-time PPG wave tracing systolic peaks and dicrotic notches, interval $\Delta t$, and formula $\text{BPM} = 60 / \Delta t$.
5. **No Clutter**:
   - Removed stick figures, human illustrations, and extra text. Clean hardware and physics only.

---

## 🚀 Technical Stack

- **Zero Build Dependencies**: Pure HTML5, CSS3, Vanilla JavaScript.
- **Three.js**: Photorealistic 3D watch rendering and exploded animation (`watch.glb`).
- **Canvas 2D API**: High-performance 60 FPS physics engines for MEMS inertial deflection and hemodynamic optical scattering.

---

## 🛠️ Explaining the Code to Hackathon Judges

- **Why MEMS?** *"Smartwatches do not see your feet. They measure inertial deflection of a microscopic silicon mass. As distance $d$ shifts, capacitance $C = \varepsilon A / d$ generates voltage proportional to acceleration."*
- **Why Green Light?** *"Oxygenated hemoglobin strongly absorbs the ~530 nm green spectrum. By shining green LEDs and measuring backscattered light with a photodiode, the watch tracks blood volume dips with each heartbeat without touching the bloodstream."*
