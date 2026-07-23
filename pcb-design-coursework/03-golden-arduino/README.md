# Board 3 — Golden Arduino (Custom ATmega328 Board)

**Course:** ECEN 3730 – PCB Design, University of Colorado Boulder

## Overview

Designed, built, and bootloaded a custom Arduino-equivalent board (a "Golden Arduino") around a bare ATmega328, then benchmarked its switching noise and rise/fall times directly against a commercial Arduino board using a shared switching-noise test shield.

## What Was Done

- Designed the schematic and PCB layout for a full custom ATmega328 board, including USB-to-UART (CH340G) and a 12 MHz crystal oscillator
- Fabricated, assembled, and bootloaded the board
- Debugged an initial USB communication failure: traced it past the USB routing and crystal oscillator (swapped, no fix) to a faulty CH340G chip — replacing the CH340G resolved it
- Connected a switching-noise test shield to both the commercial Arduino and the Golden Arduino and ran identical test code on each
- Measured rise/fall times at quietLOW, quietHIGH, and the 5V rail, with and without a noise "slammer" load, on both boards

## Key Results

| Rise/Fall Metric | Commercial | Golden Arduino | Ratio (Golden ÷ Commercial) |
|---|---|---|---|
| quietLOW Rise | 4.842 ns | 5.023 ns | 1.03× |
| quietLOW Fall | 4.276 ns | 4.862 ns | 1.13× |
| quietHIGH Rise | 5.085 ns | 5.117 ns | 1.00× |
| quietHIGH Fall | 4.245 ns | 4.928 ns | 1.16× |
| 5V Rail Rise | 4.925 ns | 4.550 ns | 0.92× |
| 5V Rail Fall | 4.349 ns | 5.115 ns | 1.17× |
| quietHIGH Rise (w/ slammer) | 20.723 ns | 20.812 ns | 1.00× |
| quietHIGH Fall (w/ slammer) | 7.894 ns | 8.973 ns | 1.13× |
| 5V Rail Rise (w/ slammer) | 20.723 ns | 22.063 ns | 1.10× |
| 5V Rail Fall (w/ slammer) | 7.928 ns | 8.980 ns | 1.13× |

The Golden Arduino ran slightly slower than the commercial board on 8 of 10 measurements (roughly 3–17% slower). Noise levels were comparable between the two boards.

**Note:** the 5V Rail Rise row is the one exception — the Golden Arduino measured *faster* (4.550 ns vs. 4.925 ns, a 0.92× ratio) than the commercial board there. The original write-up's summary table calls this "0.92x worse," which doesn't match the direction of that number — worth a quick correction before this goes in the portfolio.

## Root Cause / What I'd Do Differently

Three design decisions were identified as likely contributors to the slower performance:

- **Board size:** generous spacing was used throughout (first time designing a board this complex), which likely increased trace lengths and parasitic inductance
- **Component placement:** key components were placed farther apart than on a commercial layout, adding trace inductance
- **Decoupling capacitors:** more were added than strictly necessary, which may have increased inrush current and noise rather than reducing it

These findings directly informed the approach on Board 4 and the planned Golden Arduino V2.

## Figures
<img width="2483" height="2114" alt="IMG_0718" src="https://github.com/user-attachments/assets/845b5e19-fd73-4ace-86f0-7d8769d91745" />
*Golden Arduino, custom ATmega328 board, powered up and running the comparison test code.*

## Full Report

Full board report (with Altium schematic/layout and all scope captures) to be added to this folder.
