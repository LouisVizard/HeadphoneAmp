# 🎸 Guitar Headphone Amplifier

**A compact, single-supply headphone amplifier for electric guitar, built around a paralleled NE5532 output stage**

[![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)](https://louisvizard.github.io/Portfolio/project-pages/Headphone-Amp/headphone_amp.html)
[![Op-Amp](https://img.shields.io/badge/Op--Amp-NE5532-blue?style=for-the-badge)](https://www.ti.com/product/NE5532)
[![Power](https://img.shields.io/badge/Power-9V%20DC-yellow?style=for-the-badge)]()
[![PCB](https://img.shields.io/badge/PCB-2%20Layer-red?style=for-the-badge)]()
[![Tool](https://img.shields.io/badge/Tool-KiCAD-informational?style=for-the-badge)](https://www.kicad.org/)

---

## 📌 Overview

The **Guitar Headphone Amplifier** is a custom PCB designed to drive low-impedance headphones directly from an electric guitar signal — no computer audio interface required. The board operates from a single 9V supply using a virtual ground at VCC/2, and features a paralleled op-amp output stage capable of cleanly driving 32Ω loads without a discrete transistor buffer stage.

> Hardware design is complete. The board was designed, simulated, and laid out in full at Cornell University in 2022.

---

## ⚡ Specifications

| Parameter | Value |
|---|---|
| **Op-Amp** | NE5532 (×4, paralleled output stage) |
| **Power Supply** | 9V DC via barrel jack |
| **Input** | Electric guitar, mono |
| **Output** | 3.5mm headphone jack |
| **Volume Control** | 100kΩ potentiometer |
| **PCB Stackup** | 2-layer, KiCAD |

---

## 🏗️ Signal Chain

```
┌──────────────┐     ┌──────────────────┐     ┌──────────────────────────┐     ┌──────────────┐     ┌──────────────┐
│              │     │                  │     │                          │     │              │     │              │
│  Guitar In   │────▶│  Input Buffer    │────▶│  ×4 Parallel Output      │────▶│  Volume Pot  │────▶│  Headphone   │
│  (J3)        │     │  U1A — NE5532    │     │  Stage  U2–U3 — NE5532   │     │  RV1 100kΩ   │     │  Out (J1)    │
│              │     ├──────────────────┤     │                          │     │              │     │              │
│              │     │  Active Filter   │     │                          │     │              │     │              │
│              │     │  U1B — NE5532    │     │                          │     │              │     │              │
└──────────────┘     └──────────────────┘     └──────────────────────────┘     └──────────────┘     └──────────────┘
```

A virtual ground at VCC/2 enables single-supply operation. Coupling capacitors block DC from the headphone output.

---

## 🔧 Design Details

### Op-Amp Selection — NE5532 over LM358

The initial design used an **LM358**. Transient simulation in LTSpice exposed a critical flaw: crossover distortion appearing at frequencies as low as 5–10 kHz. This notching artifact near the zero-crossing is clearly audible and disqualifies the LM358 from audio signal paths.

The pin-compatible **NE5532** was substituted and resolved the issue entirely:

- Industry-standard audio op-amp with input voltage noise of **5 nV/√Hz**
- Significantly higher slew rate than the LM358
- Simulation confirmed clean, undistorted output across the full **20 Hz – 20 kHz** audio band

### Paralleled Output Stage

Driving 32Ω headphones requires more output current than a single op-amp section can supply without distortion. Rather than adding a discrete BJT output stage, **four NE5532 sections are wired in parallel**:

- Multiplies available output current by **4×**
- Averages individual op-amp noise contributions
- Eliminates bias complexity associated with class-AB transistor stages

### PCB Layout

The 2-layer board was laid out in KiCAD with the following considerations:

- Copper pour ground plane on the bottom layer for noise rejection
- Decoupling capacitors placed directly at each op-amp supply pin
- Ferrite bead (FB1) and protection diode (D2) at the audio output
- All panel connectors (barrel jack, potentiometer, audio jacks) placed at board edges for clean enclosure mounting

---

## 🗺️ Roadmap

- [x] Schematic capture and component selection
- [x] LTSpice simulation — LM358 vs NE5532 comparison
- [x] PCB layout and design rule check
- [ ] Bench measurement of THD+N and SNR; comparison against simulation
- [ ] Baxandall tone control in place of simple volume potentiometer
- [ ] Laser-cut or 3D-printed enclosure

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Schematic & PCB** | KiCAD |
| **Simulation** | LTSpice |
| **Core Component** | NE5532 audio op-amp |

---

## 👤 Author

**Luis Martinez**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/luis-martinez-127696165)
[![GitHub](https://img.shields.io/badge/GitHub-LouisVizard-181717?style=flat-square&logo=github)](https://github.com/LouisVizard)
[![Portfolio](https://img.shields.io/badge/Portfolio-View-FF5722?style=flat-square&logo=google-chrome)](https://louisvizard.github.io/Portfolio/)

---

*Built with KiCAD · LTSpice · NE5532*