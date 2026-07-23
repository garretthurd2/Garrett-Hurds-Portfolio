# Cascaded Boost Converter

**Course:** ECEN 4517 – Power Electronics Laboratory, University of Colorado Boulder
**Team:** Garrett Hurd, Christian Lopez

## Overview

Designed and built a two-stage (cascaded) boost converter to step the battery's ~12 V up to 150+ V, driven by a discrete analog PWM gate driver built around the SG3525A IC. This high-voltage DC bus feeds the [Stage 04 DC-AC inverter](../04-dc-ac-inverter).

## What Was Done

- Designed an analog PWM gate driver circuit (SG3525A) to generate the two gate signals needed for the cascaded boost topology
- Designed and wound two custom inductors (L1, L2) on N95 ferrite cores for the two boost stages
- Built the full two-stage power stage on perfboard
- Bench-tested the converter at its target 150 V output, measuring input/output power, semiconductor voltages, and inductor currents
- Analyzed efficiency and identified wiring-loop inductance (ringing) as the primary loss mechanism limiting efficiency below the ~80% target

## Key Results

| Metric | Result |
|---|---|
| Target / achieved output voltage | 150 V target → 153.2 V achieved |
| Converter efficiency | 76.12% (input 108.7 W @ 12.02 V, 9.04 A → output 82.73 W @ 153.2 V, 0.54 A) |
| Gate driver switching frequency | 93 kHz (target 100 kHz; RT/RD resistors were adjusted from the calculated values to hit the duty cycle needed for 150 V out) |
| Q1 / Q2 drain-source voltage | Q1: 43.2 V measured vs. 42.43 V calculated; Q2: 164 V measured vs. 150 V calculated (high-side ringing observed up to 216 V) |
| L1 / L2 measured inductance | L1: 36 µH (calc. 30.4 µH); L2: 360 µH (calc. 380 µH, 500 µH chosen part) |

**Open item:** the report notes an average L1 inductor current of 9 A in the write-up, which doesn't reconcile with the oscilloscope capture itself (3.01 A mean, 2.88 A pk-pk — consistent with the calculated 1.44 A ripple, just not the stated average). Worth a quick recheck against the original lab notes before this goes in the final write-up.

## Figures

<img width="2918" height="2115" alt="IMG_0736" src="https://github.com/user-attachments/assets/f4a22e41-9155-4c54-92b6-80cb9fab36b1" />

*Two-stage boost power stage, constructed and bench-tested.*


## Full Report

Full lab report (PDF) to be added to this folder.
