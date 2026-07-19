# PCB Design & Manufacturing Portfolio

Four boards designed, fabricated, assembled, and brought up over one semester at CU Boulder — starting with a discrete 555 timer circuit and ending with a custom 4-layer instrumentation board. Full development flow on every board: schematic capture, component selection, layout to fab-house DFM constraints, hand assembly, and bring-up.

**Tools:** Altium Designer · LTSpice · soldering iron and a lot of flux

| Board | What it is | Key skill demonstrated |
|---|---|---|
| [01 — 555 Timer Astable Oscillator](01-555-timer-astable-oscillator/) | 555 timer driving five LEDs through varying load resistances to validate a ~500Hz, 50% duty cycle square wave | First fab cycle, discrete analog design, test-point-driven validation |
| [02 — Signal Integrity: Good vs. Bad Design](02-signal-integrity-good-vs-bad-design/) | Identical hex inverter circuit built two ways — with decoupling caps + return plane, and without — to directly measure the effect on rise/fall time and noise | Decoupling, return planes, comparative scope measurement |
| [03 — Golden Arduino](03-golden-arduino/) | Custom ATmega328 Arduino-compatible board, bootloaded from bare silicon, benchmarked head-to-head against a commercial Arduino for switching noise | ISP bootloading, mixed-signal layout, comparative benchmarking |
| [04 — Golden Arduino V2 + Instrument Droid](04-golden-arduino-v2-instrument-droid/) | 4-layer board pairing a redesigned Golden Arduino with "Instrument Droid" — a DAC/op-amp/MOSFET system that measures the Thevenin voltage and resistance of an unknown source | 4-layer design, mixed-signal instrumentation, systematic debugging |

## Highlight: Instrument Droid

The most complex board of the four: a 4-layer design combining a refined second-generation Golden Arduino with a custom measurement instrument built around a DAC, op-amp, and MOSFET, controlled by an ATmega48A. Instrument Droid characterizes an unknown voltage source by sweeping current and calculating its Thevenin voltage and resistance — then validating the result against ground truth. Tested against a function generator with a known 50Ω source resistance, it measured **48Ω**.

Bring-up wasn't smooth: the board initially failed to program at all, throwing an `avrdude: stk500_recv()` error. Root-causing it took hours — the fix turned out to be a single missing 5V trace connection in the layout that had left part of the board running at 3.3V instead of 5V.

## What Four Boards Taught Me

- **Review the layout before generating Gerbers, every time.** The Instrument Droid bring-up failure traced back to a single missing power trace that a careful design review would have caught before fab.
- **Test points aren't optional.** Debugging Instrument Droid would have been faster with more test points placed up front — this shaped how test points were planned on later boards.
- **More space isn't automatically better.** Golden Arduino's extra board size and increased distance between components were meant to make routing easier, but likely introduced more trace inductance and hurt switching performance compared to a commercial board.
- **Decoupling capacitors have a right amount.** Over-adding them on Golden Arduino may have contributed to inrush current and noise rather than reducing it — good practices still need to be applied judiciously, not maximally.
- **Comparative, quantified testing beats "it looks like it works."** Measuring rise/fall times side-by-side (Board 02) and validating Instrument Droid against a known 50Ω reference (Board 04) turned "I think this works" into a number that either confirms it or doesn't.
