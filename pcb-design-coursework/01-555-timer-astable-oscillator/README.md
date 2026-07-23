# Board 1 — 555 Timer Astable Oscillator

**Course:** ECEN 3730 – PCB Design, University of Colorado Boulder

## Overview

First PCB design of the course: a 555 timer configured in astable mode to generate a square wave at a target 500 Hz with a ~50% duty cycle, driving indicator LEDs through three different series resistances (50 Ω, 1 kΩ, 10 kΩ) to visually confirm current scaling.

## What Was Done

- Designed the 555 astable oscillator circuit, selecting resistor/capacitor values to target 500 Hz at ~50% duty cycle
- Captured the schematic and PCB layout in Altium
- Fabricated and assembled the board
- Placed test points (TP1–TP3) during design to allow direct verification of the power rail, oscillator output, and LED branch voltage
- Validated the build on the bench with an oscilloscope

## Key Results

| Test point | What it confirms | Result |
|---|---|---|
| TP1 | 5 V power rail | Confirmed 5 V present |
| TP2 | 555 timer output (frequency & duty cycle) | Matched design target closely (~500 Hz, ~50% duty cycle) |
| TP3 | Voltage across the 1 kΩ LED resistor | Confirmed expected voltage drop |
| LED brightness (50 Ω / 1 kΩ / 10 kΩ) | Current scaling across the three branches | Brightness decreased with increasing resistance, as expected |

*Note: the report describes TP2's measurements as "close to the desired values" without stating the exact measured Hz/duty-cycle numbers — if you want those precise figures in here, they'd need to be read off the scope capture directly.*

## What I'd Do Differently

One soft error: the 555 timer's routing could have been cleaner. No hard errors. This is directly reflected in the routing approach on Board 2.

## Images

<img width="2371" height="1425" alt="IMG_0648" src="https://github.com/user-attachments/assets/156560da-0789-4ecc-9d82-27f1b3367d2c" />

*Assembled board, 555 timer driving indicator LEDs at three series resistances.*

## Full Report

Full board report, with Altium schematic/layout and all scope captures, and Altium files are include in this folder.
