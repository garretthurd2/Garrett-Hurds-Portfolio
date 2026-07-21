# Board 2 — Good vs. Bad Design Practices

**Course:** ECEN 3730 – PCB Design, University of Colorado Boulder

## Overview

Two identical hex-inverter circuits on the same board, laid out with opposite design philosophies, to directly measure the signal-integrity cost of skipping good PCB practices. The "good" side uses a decoupling capacitor placed close to the IC, short traces, and a return plane. The "bad" side uses a decoupling capacitor placed far from the IC, long traces, and no return plane. A shared 555 timer (reused from [Board 1](../board-01-555-timer-astable-oscillator)) drives a switch to route the same signal into either side for comparison.

## What Was Done

- Verified the reused 555 astable timer circuit (~50% duty cycle, ~500 Hz) still functioned correctly on this board
- Built two electrically identical hex-inverter circuits side by side — one with good layout practices, one with bad
- Probed the high and low outputs of each hex inverter with an oscilloscope
- Measured and compared rise time, fall time, and noise between the two designs

## Key Results

**Rise/fall time comparison (measured at the hex inverter outputs):**

| High Output | Rising Time | Falling Time |
|---|---|---|
| Good Design | 2.067 ns | 1.628 ns |
| Bad Design | 3.333 ns | 1.241 ns |
| Bad ÷ Good | 1.61× | 0.76× |

| Low Output | Rising Time | Falling Time |
|---|---|---|
| Good Design | 2.127 ns | 1.639 ns |
| Bad Design | 3.542 ns | 1.324 ns |
| Bad ÷ Good | 1.67× | 0.81× |

- Rise times: the bad-design side was consistently ~1.6–1.7× slower than the good-design side, as expected
- Noise: both high and low outputs showed visibly more noise on the bad-design side
- **Open item:** the fall-time ratios above are below 1×, meaning the bad-design side's fall time actually measured *faster* than the good design's — the opposite of the "increase in both rise and fall times" conclusion stated in the original write-up. Worth a recheck of the raw scope data before finalizing this comparison for the portfolio.

## Known Issue

SW2 (used to switch the 555 output between the good and bad hex-inverter sides) intermittently let both sides light up at once rather than switching cleanly — attributed to a soldering issue on that switch, not a design flaw.

## Figures

![Board 2 assembled, good and bad design sides](images/01-board-photo.jpeg)
*Assembled board — identical hex-inverter circuits, good design practices on one side, bad on the other.*

![Good design hex inverter output](images/02-good-design-output.png)
*Hex inverter output, good-design side — clean transitions, minimal noise.*

![Bad design hex inverter output](images/03-bad-design-output.png)
*Hex inverter output, bad-design side — visibly more noise on the same signal.*

## Full Report

Full board report (with Altium schematic/layout and all scope captures) to be added to this folder.
