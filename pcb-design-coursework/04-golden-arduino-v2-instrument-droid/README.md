# Board 4 — Instrument Droid (+ Golden Arduino V2)

**Course:** ECEN 3730 – PCB Design, University of Colorado Boulder

## Overview

A 4-layer board combining two designs: the top layer is Golden Arduino V2, a revised version of the [Board 3](../board-03-golden-arduino) custom ATmega328 board built around the lessons learned there. The bottom layer is "Instrument Droid", an intelligent measurement system (DAC, op-amp, MOSFET, ATmega48A) that characterizes an unknown voltage source's Thevenin voltage and resistance as a function of output current.

## What Was Done

- Redesigned Golden Arduino as V2: smaller board size, shorter traces to key components, moved to a 4-layer stack, removed the 3.3V LDO used in Board 3
- Designed Instrument Droid on the board's bottom layer: DAC + op-amp + MOSFET stage driven by an ATmega48A, sweeping output current to characterize a voltage source
- Added 3 addressable smart LEDs and header pins for a planned future OLED readout
- Debugged a hard failure (`avrdude: stk500_recv(): programmer is not responding`) that blocked all code uploads. Traced past the CH340G and TVS diode (both swapped, no fix) to the actual root cause: separate 5V nets in the layout were never tied together, starving the ATmega's TXD/RXD at ~3.3V instead of 5V. Fixed with a bodge wire tying the nets together.
- Validated Instrument Droid against three known voltage sources: a 5V wall jack, a 9V DC power supply, and a 5V function generator with a known 50 Ω Thevenin resistance

## Key Results

| Metric | Result |
|---|---|
| Thevenin resistance accuracy | Measured ~48 Ω on a function generator with a known 50 Ω source resistance — ~4% error |
| Root-cause debugging | Isolated a board-killing net-connectivity issue via component-swap elimination + TA collaboration, fixed with a single bodge wire |
| Design iteration | Reduced board footprint and trace length vs. Board 3, per that board's own post-mortem |

## What I'd Do Differently

- **Review the layout against the schematic before generating Gerbers** — the disconnected 5V nets were a layout oversight that a careful pre-fab review would have caught, and cost hours of debugging
- **Add more test points** — the board had too few to make isolating the root cause fast; more test points would have shortened the debug cycle significantly

## Figures

<img width="1900" height="1482" alt="IMG_0766" src="https://github.com/user-attachments/assets/439b3930-62ce-4d63-9a26-a6564482da2e" />

<img width="2325" height="1723" alt="IMG_0767" src="https://github.com/user-attachments/assets/2fde15c9-3858-489b-839e-cac14fc80a22" />

*Assembled 4-layer board — Golden Arduino V2 on top, Instrument Droid underneath, with the smart LED addition.*

## Full Report

Full board report, with Altium schematic/layout and all scope captures, and Altium Gerber files are include in this folder.
