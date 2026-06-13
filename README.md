# Pulse Code Modulation (PCM) Pipeline: KiCad Simulation

## 📌 Overview
This repository contains a complete, functional KiCad SPICE simulation of a Pulse Code Modulation (PCM) pipeline. It demonstrates the fundamental analog-to-digital conversion process, including continuous-time sampling, voltage quantization, and 3-bit digital encoding.

This project is highly useful for understanding discrete signal processing and utilizing behavioral voltage sources in SPICE to simulate complex analog-to-digital boundaries.

---

## ⚙️ Circuit Architecture
The simulation pipeline is broken down into three primary stages:
1. **Sample & Hold (S/H):** An N-Channel MOSFET acts as a high-speed sampling switch, driven by a 10kHz clock pulse, holding the instantaneous voltage of a 1kHz analog sine wave across a capacitor.
2. **Quantization:** The continuous held voltage is mapped to discrete thresholds to create a staircase approximation.
3. **Encoding (3-Bit):** Behavioral SPICE Voltage Sources utilize analog `sgn()` math functions to encode the quantized levels into three distinct digital logic streams (MSB, Bit 1, and LSB).

---

## 📊 Simulation Details
The project is configured for a **Transient Analysis** to capture the time-domain conversion sequence. 

**Key Observational Note:** To ensure visual clarity in the SPICE plotter, the peak logic voltages for the encoded bits are deliberately staggered (e.g., 4.8V, 5.0V, 5.2V). This prevents the digital traces from completely eclipsing one another when they toggle states simultaneously.

---

## 🛠️ Software Requirements
* **KiCad EDA** (v6.0 or higher recommended)
* Integrated `ngspice` simulator (included by default in KiCad)

---

## 🚀 How to Run the Simulation
1. Clone this repository to your local machine:
   ```bash
   git clone [https://github.com/satvikpandurangi/Pulse-Code-Modulation-Simulation-kicad.git](https://github.com/satvikpandurangi/Pulse-Code-Modulation-Simulation-kicad.git)
   ```
2. Open the **`.kicad_pro`** project file in the KiCad Project Manager.
3. Open the **Schematic Editor** to view the circuit layout.
4. Go to **Inspect > Simulator** in the top menu.
5. Click the **Run/Stop Simulation** (Play) button.
6. Probe the input sine wave, the S/H stage, and the final 3-bit logic outputs to observe the conversion.

---

## 📂 Repository Contents
* **`.kicad_pro`** - Main project file
* **`.kicad_sch`** - The schematic wiring and SPICE directives
* **`.kicad_pcb`** - Blank PCB layout file (ready for physical routing if desired)

> *Created for educational purposes and open-source hardware exploration.*
