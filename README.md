# Guitar Headphone Amplifier

A compact, single-supply headphone amplifier designed for electric guitar, built around the NE5532 audio op-amp. Developed at Cornell University in 2022.

---

## Overview

This project delivers a full-chain amplifier that accepts a standard electric guitar signal and drives low-impedance headphones (≈32Ω) with adjustable volume. The design covers the complete engineering workflow: requirements, schematic capture, SPICE simulation, PCB layout, and hardware bring-up.

---

## Specifications

| Parameter | Value |
|-----------|-------|
| Op-Amp | NE5532 (×4, paralleled output stage) |
| Power Supply | 9 V DC via barrel jack |
| Input | Mono electric guitar (3.5 mm / ¼" jack) |
| Output | 3.5 mm headphone jack |
| Volume Control | 100 kΩ potentiometer |
| PCB | 2-layer, designed in KiCAD |

---

## Signal Chain

```
Guitar In (J3)
    →  Input Buffer     [U1A — NE5532]
    →  Active Filter    [U1B — NE5532]
    →  ×4 Parallel Output Stage  [U2–U3 — NE5532]
    →  Volume Pot       [RV1 — 100 kΩ]
    →  Headphone Out    [J1]
```

A virtual ground at VCC/2 enables single-supply operation. Coupling capacitors block DC from the headphone output.

---

## Design Decisions

### Op-Amp Selection: NE5532 over LM358

The initial prototype used an LM358. Transient simulation in LTSpice revealed crossover distortion beginning at 5–10 kHz — a notching artifact near the zero-crossing that is clearly audible on headphones. The LM358 is optimized for DC and low-frequency applications and is not suitable for audio signal paths.

The pin-compatible **NE5532** was substituted:

- Industry-standard audio op-amp
- Input voltage noise: **5 nV/√Hz**
- Significantly higher slew rate
- Simulation confirmed clean output across the full 20 Hz – 20 kHz audio band

### Paralleled Output Stage

Driving 32 Ω headphones requires more output current than a single op-amp section can cleanly supply. Rather than adding a discrete BJT output stage, **four NE5532 sections are wired in parallel**:

- Multiplies available output current by 4×
- Averages individual op-amp noise contributions
- Avoids additional components and bias complexity

### PCB Layout

The 2-layer board was laid out in KiCAD with the following considerations:

- Copper pour ground plane on the bottom layer for noise rejection
- Decoupling capacitors placed directly at each op-amp supply pin
- Ferrite bead (FB1) and protection diode (D2) at the audio output
- Connectors (barrel jack, potentiometer, audio jacks) placed at board edges for panel mounting

---

## Tools

- **KiCAD** — Schematic capture and PCB layout
- **LTSpice** — AC and transient simulation

---

## Future Work

- Bench measurement of THD+N and SNR; comparison against simulation
- Baxandall tone control in place of a simple volume potentiometer
- Microcontroller integration for digital signal processing
- Laser-cut or 3D-printed enclosure

---

## Author

**Luis Martinez**
[Portfolio](https://louisvizard.github.io/Portfolio/) · [LinkedIn](https://www.linkedin.com/in/luis-martinez-127696165) · [GitHub](https://github.com/LouisVizard) · manilumart@gmail.com