# 01 — 555 Timer Astable Oscillator

*ECEN 3730 — Garrett Hurd*

## Project Overview

The goal of this board is to set up a 555 timer that is in astable mode and have it produce a square wave with a duty cycle of about 50% and have a frequency of about 500 Hz. To prove that this board and 555 timer will work, we will drive LEDs with resistances of series 50 Ohms, 1k Ohms and 10K Ohms. If the LEDs are decreasing in brightness at higher resistances, then this proves that the board design works.

The following images are the block diagram and my initial sketch of the 555 timer, my Altium schematic, my PCB layout in Altium, and then my physical board working.

**Fig. 1: Block and Circuit Diagram**
![Block and circuit diagram](images/01-block-diagram.jpg)

**Fig. 2: Altium Schematic**
![Altium schematic](images/02-schematic.png)

**Fig. 3: PCB Layout in Altium**
![PCB layout](images/03-pcb-layout.png)

**Fig. 4: Physical Board**
![Physical board](images/04-physical-board.jpg)

**Fig. 5: Board soldered and powered up**
![Board soldered and powered up](images/05-board-powered-up.jpg)

## What Worked

Based on the photos above, my first board works! The indicator LED and respective resistor LEDs are on which proves to me that my board is functioning. However, just having the LED indicators on isn't enough. I need to know if my timer circuit is functioning as it should. To do this I should measure the board at the test points I placed during the design process.

The following are scope captures at those test points.

**Fig. 6: 5V Test Point**
![Scope capture, TP1 5V](images/06-scope-tp1-5v.png)

Above is the scope capture at TP1, this is just showing and confirming that 5V is powering the board.

**Fig. 7: 555 Output**
![Scope capture, TP2 555 output](images/07-scope-tp2-555-output.png)

Above is the scope capture at TP2, which is the output of my 555 timer. On this scope capture I added measurements to confirm the duty cycle and frequency. As you can see, these values are close to the desired values. This proves that my design of the 555 timer is working and is close to the values I designed it for.

**Fig. 8: 1K Resistor Output**
![Scope capture, TP3 1k output](images/08-scope-tp3-1k-output.png)

Above is the output of TP3, this shows the voltage across the 1k resistor. With the scope captures above, I am very confident my board design is functioning as it should. There is nothing to report about anything that did not work.

## Analysis of the Project

I am very happy with my first PCB! While I followed Professor Bogatin's videos to understand how Altium works and learned the foundations of PCB design, I took creative freedom in the 555 timer circuit resistor values to get as close as I can to a 50% duty cycle and 500 Hz frequency. In future designs I want to use more creative freedom with schematic design and PCB layouts. There is one soft error in my PCB design, which is the routing of my 555 timer. This tracing could have looked a lot better; I will use this experience to route better in future board designs. I don't have any hard errors to report for this board.

Overall, this first board was a great introduction to this class and PCB design as a whole. I have already taken the learning experience from this board and have applied it to the next board I have designed.

---

Next: [02 — Signal Integrity: Good vs. Bad Design](../02-signal-integrity-good-vs-bad-design/)
