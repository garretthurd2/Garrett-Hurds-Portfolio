# 02 — Signal Integrity: Good vs. Bad Design Practices

*ECEN 3730 — Garrett Hurd*

## Project Overview

The goal of this board is to compare good and bad design practices with identical circuit designs. Good design practices include a decoupling capacitor close to the IC, short traces, and a return plane. Bad design practices include a decoupling capacitor far from the IC, long traces, and not having a return plane. To see the effects of good and bad design practices, I compared the rise and fall times of each design and noted the differences.

**Fig. 1: Block Diagram of Board 2**
![Block diagram](images/01-block-diagram.jpg)

**Fig. 2: Altium Schematic**
![Altium schematic](images/02-schematic.png)

**Fig. 3: PCB Layout in Altium**
![PCB layout](images/03-pcb-layout.png)

**Fig. 4: Physical Board**
![Physical board](images/04-physical-board.jpg)

**Fig. 5: Board soldered and powered up**
![Board soldered and powered up](images/05-board-powered-up.jpg)

## What Worked

Before determining the difference between good and bad design practices, I needed to confirm my timer circuit was working. This timer circuit is the same from Board 1 — astable mode, duty cycle of about 50%, and frequency of about 500 Hz. I used 1k Ohms for RA instead of 300 Ohms this time.

**Fig. 6: 555 Timer Output**
![555 timer output](images/06-555-output.png)

The timer circuit is working as it should based on the measurements. Time to move to the analysis. First, the good-design side of the board: Channel 1 (yellow) probes TP12 (the scope output of the hex inverter), and Channel 2 (green) probes TP9 and TP8 (high and low output of the hex, respectively).

**Fig. 7: Good Design High Rising Output**
![Good design high rising](images/07-good-high-rising.png)

**Fig. 8: Good Design High Falling Output**
![Good design high falling](images/08-good-high-falling.png)

**Fig. 9: Good Design Low Rising Output**
![Good design low rising](images/09-good-low-rising.png)

**Fig. 10: Good Design Low Falling Output**
![Good design low falling](images/10-good-low-falling.png)

With a decoupling capacitor near the hex inverter and a return plane in place — good design practices — there is little to no noise on either the high or low outputs. Now compare the same outputs with bad design practices.

**Fig. 11: Bad Design High Rising Output**
![Bad design high rising](images/11-bad-high-rising.png)

**Fig. 12: Bad Design High Falling Output**
![Bad design high falling](images/12-bad-high-falling.png)

**Fig. 13: Bad Design Low Rising Output**
![Bad design low rising](images/13-bad-low-rising.png)

**Fig. 14: Bad Design Low Falling Output**
![Bad design low falling](images/14-bad-low-falling.png)

Bad design practices have a major effect on signal integrity. There is significantly more noise on both the high and low outputs compared to the good design, along with an increase in both rise and fall times.

Based on all the scope shots, the board is working as intended to demonstrate the difference between good and bad design practices.

## Analysis of the Project

Using the scope shots above, I compared the rise and fall times of both the high and low outputs of the hex inverters.

| High Output | Rising Time | Falling Time |
|---|---|---|
| Good Design | 2.067 ns | 1.628 ns |
| Bad Design | 3.333 ns | 1.241 ns |
| **Difference** | **1.61x slower** | **0.762x slower** |

| Low Output | Rising Time | Falling Time |
|---|---|---|
| Good Design | 2.127 ns | 1.639 ns |
| Bad Design | 3.542 ns | 1.324 ns |
| **Difference** | **1.67x slower** | **0.808x slower** |

Bad design practices measurably hurt the performance of an otherwise-identical circuit. Incorporating good design practices — even something as simple as a decoupling capacitor and a return plane — has a real, quantifiable effect on signal integrity.

For this board, I also took what I learned from Board 1 and applied it here. I routed traces and placed components a lot better this time, and learned how powerful and useful net labels are in Altium.

**Hard errors:** I had an issue with SW2, the switch used to route the 555 timer's output to either the good or bad hex design side. Sometimes both sides would light up at once, other times it worked as intended. I believe this was a soldering issue with the switch — in future designs I'll be extra careful soldering switch elements. No soft errors to report.

Overall, this board taught me basic and powerful design principles that I've carried into every board since.

---

Previous: [01 — 555 Timer Astable Oscillator](../01-555-timer-astable-oscillator/)
Next: [03 — Golden Arduino](../03-golden-arduino/)
