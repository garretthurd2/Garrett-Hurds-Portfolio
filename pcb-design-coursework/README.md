# PCB Design Coursework

**Course:** ECEN 3730 – PCB Design, University of Colorado Boulder

Four boards, each building directly on lessons from the last — from a first 555 timer layout through a custom ATmega328 board benchmarked against a commercial Arduino, to a 4-layer board combining a revised microcontroller design with a real measurement instrument. Each board below is a standalone write-up — click through for the full breakdown, key results, and figures. Full board reports (Altium schematics/layouts) are included in each board's folder.

| Board | What it is | Key result |
|---|---|---|
| [01 — 555 Timer Astable Oscillator](01-555-timer-astable-oscillator) | First board: a 555 timer astable oscillator (~500 Hz, ~50% duty cycle) driving LEDs at three series resistances | Confirmed working at all three test points; one soft routing error noted for next time |
| [02 — Good vs. Bad Design Practices](02-good-vs-bad-design-practices) | Two identical hex-inverter circuits, one built with decoupling caps/short traces/a return plane, one without | Bad-design side ran 1.6–1.7× slower on rise time with visibly more noise on identical circuitry |
| [03 — Golden Arduino](03-golden-arduino) | A custom ATmega328 board benchmarked head-to-head against a commercial Arduino for switching noise and rise/fall time | Ran 3–17% slower on 8 of 10 metrics; root-caused to board size, component spacing, and excess decoupling caps |
| [04 — Instrument Droid](04-instrument-droid) | A 4-layer board pairing a revised Golden Arduino V2 with "Instrument Droid," a DAC/op-amp/MOSFET system that measures a voltage source's Thevenin resistance | Measured 48 Ω on a known 50 Ω source (~4% error); found and fixed a board-killing net-connectivity bug |

## Progression

Each board's post-mortem fed directly into the next: Board 1's routing error informed Board 2's cleaner layout; Board 3's oversized, sparsely-populated design led directly to Board 4's smaller, denser 4-layer redesign; Board 4's missing-test-point lesson is the next thing to apply going forward.
