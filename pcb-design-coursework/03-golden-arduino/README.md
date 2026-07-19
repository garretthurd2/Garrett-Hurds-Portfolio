# 03 — Golden Arduino

*ECEN 3730 — Garrett Hurd*

## Introduction

The purpose of Board 3 is to design, build, and bootload my own Arduino board using the ATmega328 — called a Golden Arduino — and compare it to a commercial Arduino board. To see which board is better, I analyzed the switching noise on a commercial Arduino using a switching noise shield, then did the same analysis on my Golden Arduino.

Before the analysis, here's the design process. Below is the block diagram and schematic for my Golden Arduino — I used resources online as an outline for this design.

**Fig. 1: Block Diagram for Board 3**
![Block diagram](images/01-block-diagram.jpg)

**Fig. 2: Schematic for Board 3**
![Schematic](images/02-schematic.png)

When designing the PCB, I wanted to give myself as much room as possible. There are a lot of components on this board, and a lot of routing needed for the ATmega and the USB-to-UART. The extra space helped with routing and component placement.

**Fig. 3: Layout for Board 3**
![PCB layout](images/03-pcb-layout.png)

**Fig. 4: Powered up Board 3**
![Board powered up](images/04-powered-up.jpg)

Figure 4 shows my Golden Arduino powered up, bootloaded, and running the code given for the comparison. I didn't have any issues bootloading the board — but I did have an issue communicating with my computer initially. At first I thought it was how I'd routed the USB port to the CH340G, but that wasn't it. I noticed the 12MHz crystal oscillator wasn't oscillating at all, so I replaced it — same issue. Finally, I replaced the CH340G chip and connected the board again, and it started working correctly. I was able to upload code, confirmed the board was working, and continued with the analysis.

## Analysis

Now to compare switching noise between a commercial Arduino and my Golden Arduino. I connected the provided switching noise shield to each board and uploaded the provided code.

**Fig. 5: Golden Arduino with switching noise shield**
![Golden Arduino with shield](images/05-golden-arduino-shield.jpg)

To determine which board is better, I compared switching noise on quietLOW, quietHIGH, and the board's power rail as the ATmega switches states.

**Commercial Arduino scope shots:**

Fig. 6–7: quietLOW rise/fall time and noise
![quietLOW rise](images/06-commercial-quietlow-rise.png)
![quietLOW fall](images/07-commercial-quietlow-fall.png)

Fig. 8–9: quietHIGH rise/fall time and noise
![quietHIGH rise](images/08-commercial-quiethigh-rise.png)
![quietHIGH fall](images/09-commercial-quiethigh-fall.png)

Fig. 10–11: 5V rail rise/fall time and noise
![5V rail rise](images/10-commercial-5v-rise.png)
![5V rail fall](images/11-commercial-5v-fall.png)

Fig. 12–15: quietHIGH and 5V rail rise/fall with slammer
![quietHIGH rise w/ slammer](images/12-commercial-quiethigh-rise-slammer.png)
![quietHIGH fall w/ slammer](images/13-commercial-quiethigh-fall-slammer.png)
![5V rail rise w/ slammer](images/14-commercial-5v-rise-slammer.png)
![5V rail fall w/ slammer](images/15-commercial-5v-fall-slammer.png)

**My Golden Arduino scope shots:**

Fig. 16–17: quietLOW rise/fall time and noise
![quietLOW rise](images/16-golden-quietlow-rise.png)
![quietLOW fall](images/17-golden-quietlow-fall.png)

Fig. 18–19: quietHIGH rise/fall time and noise
![quietHIGH rise](images/18-golden-quiethigh-rise.png)
![quietHIGH fall](images/19-golden-quiethigh-fall.png)

Fig. 20–21: 5V rail rise/fall time and noise
![5V rail rise](images/20-golden-5v-rise.png)
![5V rail fall](images/21-golden-5v-fall.png)

Fig. 22–25: quietHIGH and 5V rail rise/fall with slammer
![quietHIGH rise w/ slammer](images/22-golden-quiethigh-rise-slammer.png)
![quietHIGH fall w/ slammer](images/23-golden-quiethigh-fall-slammer.png)
![5V rail rise w/ slammer](images/24-golden-5v-rise-slammer.png)
![5V rail fall w/ slammer](images/25-golden-5v-fall-slammer.png)

| Rise and Fall Times | Commercial Arduino | Golden Arduino | Comparison |
|---|---|---|---|
| quietLOW Rise | 4.842 ns | 5.023 ns | 1.03x worse |
| quietLOW Fall | 4.276 ns | 4.862 ns | 1.13x worse |
| quietHIGH Rise | 5.085 ns | 5.117 ns | 1x worse |
| quietHIGH Fall | 4.245 ns | 4.928 ns | 1.16x worse |
| 5V Rail Rise | 4.925 ns | 4.550 ns | 0.92x worse |
| 5V Rail Fall | 4.349 ns | 5.115 ns | 1.17x worse |
| quietHIGH Rise w/ Slammer | 20.723 ns | 20.812 ns | 1x worse |
| quietHIGH Fall w/ slammer | 7.894 ns | 8.973 ns | 1.13x worse |
| 5V Rail Rise w/ slammer | 20.723 ns | 22.063 ns | 1.1x worse |
| 5V Rail Fall w/ slammer | 7.928 ns | 8.980 ns | 1.13x worse |

## Conclusion

Based on the table above, my Golden Arduino design came in slightly worse than a commercial Arduino in switching times. Noise was roughly comparable between the two. This wasn't the outcome I was hoping for — the goal was to beat the commercial board — but it's a genuine learning experience, and I identified specific improvements for future revisions:

1. **Board size.** I gave myself as much room as possible to place components and route traces, since I'd never designed a board this complex before. In hindsight, this is likely one of the main reasons the board underperformed.
2. **Distance between components.** The extra space meant important components sat further apart than on a commercial board — that extra distance likely added trace inductance, which meant more noise and worse performance.
3. **Decoupling capacitors.** I think I went a bit overboard here. While decoupling caps are good practice, I believe the extra ones I added contributed to more inrush current, adding noise rather than reducing it.

Overall, it was a fun challenge to design, build, and power on my first Golden Arduino — I'm proud to have a functional custom Arduino board of my own design. While performance came in slightly behind a commercial board, I learned a lot about designing complex, high-component-count boards, and used these lessons directly in Board 4.

---

Previous: [02 — Signal Integrity: Good vs. Bad Design](../02-signal-integrity-good-vs-bad-design/)
Next: [04 — Golden Arduino V2 + Instrument Droid](../04-golden-arduino-v2-instrument-droid/)
