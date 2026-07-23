# DC-AC Inverter

**Course:** ECEN 4517 – Power Electronics Laboratory, University of Colorado Boulder

**Team:** Garrett Hurd, Christian Lopez

## Overview

Designed and built a modified sine-wave inverter to convert the high-voltage DC bus from the [Stage 03 cascaded boost converter](../03-cascaded-boost-converter) into household-outlet-equivalent AC, completing the panel-to-battery-to-AC-load power path for the system.

## What Was Done

- Built a half-bridge gate driver circuit, mirrored across both halves of the output waveform
- Programmed the microcontroller's PWM timers (count-up/down mode, phase-shifted) to generate a modified (stepped) sine wave rather than a pure sinusoid
- Tuned the gate timing to produce a 60 Hz output matching standard household AC
- Bench-tested the inverter's output voltage, current, and switching duty cycle

## Key Results

| Metric | Result |
|---|---|
| Output voltage | 119.6 Vrms |
| Output current | 0.287 Arms |
| Output frequency | 60.28 Hz — matches standard 60 Hz household AC |
| Gate driver duty cycle | ~68% |

## Figures

<img width="3287" height="2803" alt="IMG_0748" src="https://github.com/user-attachments/assets/0fed7721-5d68-4559-8745-a36a7f9fb908" />

*Modified sine-wave inverter, constructed and bench-tested.*



## Full Report

Full lab report, with all calculations, scope captures & LTSpice simulations captures are include in this folder.
