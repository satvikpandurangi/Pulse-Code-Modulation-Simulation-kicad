# Pulse Code Modulation (PCM) Pipeline: KiCad Simulation

## Overview

This repository contains a KiCad schematic and SPICE simulation of a complete Pulse Code Modulation (PCM) pipeline — covering continuous-time **Sampling**, voltage **Quantization**, and 3-bit **Digital Encoding**. The transient simulation, run through KiCad's integrated `ngspice` engine, verifies each stage of the analog-to-digital conversion process.

## Features

- MOSFET-based Sample & Hold (S/H) circuit driven by a high-frequency clock
- Behavioral SPICE voltage sources implementing quantization and encoding logic
- 3-bit digital output (MSB, Bit 1, LSB) derived from an analog sine wave input
- Staggered bit-trace voltages for clear waveform visualization

## Circuit Description

The pipeline consists of three sequential phases:

- **Sampling:** An N-Channel MOSFET acts as a high-speed Sample & Hold switch, capturing the instantaneous analog input voltage and holding it across a capacitor.
- **Quantization:** The held analog voltage is mapped to the nearest discrete voltage level, producing a uniform "staircase" approximation of the original waveform.
- **Encoding:** The quantized levels are converted into a 3-bit binary digital format — Most Significant Bit (Bit 2), Bit 1, and Least Significant Bit (Bit 0) — using SPICE behavioral voltage sources with `sgn()` math functions.

## Components

| Type | Component | Value / Rating |
|------|-----------|-----------------|
| Active | N-Channel MOSFET (Sampling Switch) | — |
| Passive | Capacitor (Holding Element) | 10 nF |
| Mathematical | SPICE Behavioral Voltage Sources (B_V) | Quantization & encoding logic |
| Source | Sine Wave Voltage Source (Analog Input) | 1 kHz, 2.5 V offset, 2.5 V amplitude |
| Source | Pulse Voltage Source (Sampling Clock) | 10 kHz, 10 V amplitude, 10 µs pulse width |

## Simulation Setup

1. An N-Channel MOSFET is wired in series with a 10 nF grounding capacitor to form the Sample & Hold stage.
2. A 1 kHz sine wave (2.5 V offset, 2.5 V amplitude) drives the MOSFET Drain as the analog input.
3. A 10 kHz pulse wave (10 V amplitude, 10 µs width) drives the MOSFET Gate as the sampling clock.
4. The voltage across the capacitor is tapped at the `/HOLD` net to capture the sampled signal.
5. Three Behavioral Voltage Sources (`B_V`), using `sgn()` encoding logic, output to `/BIT2`, `/BIT1`, and `/BIT0`.
6. Bit output peaks are staggered (4.8 V, 5.0 V, 5.2 V) to keep overlapping digital traces distinguishable on the plot.
7. Transient analysis is run as `.tran 1u 3m` over a 3 ms window, plotting the input, sampled, and encoded waveforms.

## Results

| Signal | Trace Color | Description |
|--------|-------------|--------------|
| Analog Input | Red | Continuous 1 kHz sine wave varying between 0 V and 5.0 V |
| Sampled Signal (`/HOLD`) | Green | Staircase pattern — tracks the input during each 10 µs clock pulse and holds flat otherwise |
| Bit 0 / LSB (`/BIT0`) | Yellow (peak 4.8 V) | Digital square wave from the lowest quantization thresholds |
| Bit 1 (`/BIT1`) | Orange (peak 5.0 V) | Digital square wave from the mid-level quantization thresholds |
| Bit 2 / MSB (`/BIT2`) | Purple (peak 5.2 V) | Stays HIGH for the upper half of the sine wave (above 2.5 V) and LOW for the lower half |

The simulation confirms the full PCM pipeline operating correctly: the analog sine wave is sampled and held, then quantized and encoded into a 3-bit digital representation that accurately tracks the original signal's amplitude.

## Repository Structure

```
.
├── PCM.kicad_pro       # Main KiCad project file
├── PCM.kicad_sch       # Schematic and SPICE directives
├── PCM.kicad_pcb       # PCB layout file
├── PCM_Schematic.png   # Circuit schematic
├── PCM_Output.png      # Simulation waveform output
└── README.md
```

## How to Run

1. Clone this repository:
   ```bash
   git clone https://github.com/satvikpandurangi/Pulse-Code-Modulation-Simulation-kicad.git
   ```
2. Open `PCM.kicad_pro` in the KiCad Project Manager.
3. Open the Schematic Editor to view the circuit layout.
4. Go to **Inspect > Simulator** in the menu bar.
5. Click **Run/Stop Simulation** to execute the transient analysis.
6. Probe the analog input, `/HOLD`, and the `/BIT2`, `/BIT1`, `/BIT0` nets to observe the sampling, quantization, and encoding stages.

## Screenshots

**Schematic:**

![Schematic](PCM_Schematic.png)

**Simulation Output:**

![Simulation Output](PCM_Output.png)

