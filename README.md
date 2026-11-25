# Inside a Fitness Watch — Biometric Telemetry & Hardware Anatomy

> **GVH Hackathon Project**  
> An interactive single-page scientific walkthrough explaining how modern fitness watches measure steps (via MEMS accelerometer) and heart rate (via optical PPG sensors).

---

## 🌐 Live Local Server

The project runs on a zero-dependency local HTTP server:
```bash
# Start local server
python3 -m http.server 8000

# Open in browser:
http://localhost:8000
```
Or simply double-click `index.html` (or run `open index.html` on macOS).

---

## 💎 Design & Architecture

### 1. Minimalist Monochrome Aesthetic
- Black & white high-contrast technical palette (`#000000` deep black, crisp white `#ffffff`, graphite borders `#222222`).
- Clean Swiss/Apple-style technical typography with high legibility.
- Zero extraneous visual clutter.

### 2. Single Continuous Page Layout
- Both features are presented sequentially on **one single page** (no modal toggles or landing cards).
- Smooth anchor scrolling (`#sec-accel`, `#sec-ppg`, `#takeaways`).

### 3. Interactive 3D Exploded View (Three.js)
Located right in the hero section:
- Full 3D model of a fitness watch with realistic materials (sapphire crystal, AMOLED screen, PCB logic board, MEMS accelerometer chip, Li-Po battery, optical PPG sensor array, unibody ceramic housing, and straps).
- **Interactive Explode Slider**: Expand and contract the internal components along the vertical axis.
- **Mouse / Touch 360° Orbit**: Rotate the watch freely to inspect individual internal layers.

---

## 📐 Scientific Walkthrough Modules

### 01 / Step Counting — MEMS Accelerometer
1. **Movement Begins**: Dynamic wrist swing displacement during locomotion.
2. **Accelerometer Internal Structure**: Silicon proof mass ($m$), suspension springs ($k$), fixed and movable differential capacitor plates ($C = \varepsilon A / d$).
3. **Inertia in Action**: Newton’s first law causing proof mass displacement relative to housing during acceleration ($F = ma$).
4. **3-Axis Signal Acquisition**: Real-time tri-axial stream ($X$, $Y$, $Z$).
5. **Step Recognition Algorithm**: Combined acceleration vector magnitude $|a| = \sqrt{a_x^2 + a_y^2 + a_z^2}$, adaptive thresholding, low-pass cadence verification, and step counting.

### 02 / Heart Rate Monitoring — Optical Photoplethysmography (PPG)
1. **Sensor Activation**: Dual green LEDs ($\lambda \approx 530\text{ nm}$) and central photodiode on rear plate.
2. **Light Penetration**: Optical interaction with epidermis, dermis, and subcutaneous vascular beds.
3. **Cardiac Contraction**: Ventricular ejection causing systolic vessel expansion and diastolic relaxation.
4. **Modulation of Backscattered Light**: Inverse relationship between blood volume and photodiode reflection intensity.
5. **Analog Front-End (AFE)**: Transimpedance amplifier, bandpass noise filter, and 16-bit ADC conversion.
6. **PPG Waveform Anatomy**: Systolic peak, dicrotic notch (aortic valve closure), and period $\Delta t$.
7. **BPM Computation**: $\text{BPM} = 60 / \Delta t$ calculation and live AMOLED display update.

---

## 🛠️ How to Explain the Code to Hackathon Judges

When defending the project before the GVH jury:

1. **Clean Code & Zero Bloat**:
   > *"Everything runs in a single lightweight HTML file with vanilla JavaScript and WebGL/Three.js. It requires no Node.js build pipelines, no bundlers, and achieves 60 FPS smoothly on any device."*

2. **Physically Grounded Mathematical Simulation**:
   > *"The waveforms are not static images. The PPG waveform is calculated dynamically using a piecewise mathematical model reflecting true cardiac hemodynamics (systole, dicrotic notch, diastole). The accelerometer simulation models damped harmonic oscillation and tri-axial phase shifts."*

3. **Scrollytelling & Exploded View**:
   > *"We implemented a 3D exploded view to bridge the gap between external industrial design and internal physics. Judges and users can see exactly where the MEMS chip and PPG LEDs sit inside the watch."*

---

## 🚀 3D Integration Guide (for GLTF / CAD Models)

See [roadmap.md](file:///Users/ulykbek/Desktop/GVH%20-%20Fitness%20Watch/roadmap.md) for step-by-step instructions on importing downloaded `.gltf` / `.glb` models from Blender or Sketchfab.
