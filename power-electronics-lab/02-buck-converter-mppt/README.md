# Buck Converter & MPPT

**Course:** ECEN 4517 – Power Electronics Laboratory, University of Colorado Boulder

**Team:** Garrett Hurd, Christian Lopez

## Overview

Designed and built a 100 kHz buck converter to step the PV panel's output down to battery-charging voltage, then added current/voltage sensing and a duty-cycle-sweep MPPT algorithm to track the panel's peak power point in real time. Converter specs (input/output ranges, switching frequency) came directly from the panel characterization in [Stage 01](../01-pv-panel-characterization).

## What Was Done

- Designed and wound a custom inductor (PQ 26/25 core, N95 ferrite) for a 100 kHz buck converter
- Built and bench-tested the buck power stage across a range of loads (1 W to full 85 W) to characterize efficiency vs. duty cycle
- Measured capacitor ESR and estimated its contribution to converter power loss
- Designed a battery current/voltage sensing circuit (shunt + differential amp) and validated its accuracy against multimeters
- Implemented an MPPT algorithm on a microcontroller: sweeps duty cycle in 2.5% steps, averages 300 ADC samples per point, and tracks peak power with a charge-control cutoff to protect the battery
- Field-tested the full system outdoors with the actual PV panel, under both unshaded and shaded conditions

## Key Results

| Metric | Result |
|---|---|
| Buck converter efficiency at full load (85 W) | 91% |
| Buck converter efficiency at 15 W load | 95.3% |
| Buck converter efficiency at 1 W load | 48.9% — efficiency drops sharply at low duty cycle |
| Sensing circuit accuracy | 0.8% voltage error, 0.68% current error vs. multimeter |
| MPPT benchtop performance | Found 74.7 W peak power at 93% efficiency |
| MPPT field test (unshaded, 1160 W/m²) | 67.57 W avg. max power point at 97.4% efficiency — 5.6% more power than direct panel-to-battery charging |
| MPPT field test (shaded) | Anomalous — only 8.75 W found vs. 32.16 W panel capability from Stage 01; flagged as an open issue, likely tied to low battery state-of-charge during that test run |

## Figures

<img width="4266" height="3624" alt="IMG_0683" src="https://github.com/user-attachments/assets/7b549fc7-1c63-49a9-bf44-b5630a522d5d" />


*Buck converter power stage, constructed and bench-tested.*

## Full Report

Full lab report, with all calculations, scope captures & LTSpice simulations captures are include in this folder.
