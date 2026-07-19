# 04 — Golden Arduino V2 + Instrument Droid

*ECEN 3730 — Garrett Hurd*

## Introduction

The purpose of Board 4 was to build an intelligent measurement system called "Instrument Droid." This system characterizes any voltage source by measuring its Thevenin voltage and resistance as a function of output current. The system contains three key devices — a DAC, an op-amp, and a MOSFET — all controlled by an ATmega48A.

Rather than building Instrument Droid as a shield sitting on top of my Golden Arduino, I decided to remake the Golden Arduino as a 4-layer board: the top layer serves as Golden Arduino V2, and the bottom layer serves as Instrument Droid.

**Fig. 1: Block Diagram for Board 4**
![Block diagram](images/01-block-diagram.jpg)

**Fig. 2: Schematic for Board 4**
![Schematic](images/02-schematic.png)

**Fig. 3: Top View, Golden Arduino V2**
![Top view, Golden Arduino V2](images/03-top-view.jpg)

**Fig. 4: Bottom View, Instrument Droid**
![Bottom view, Instrument Droid](images/04-bottom-view.jpg)

**Fig. 5: Top View, Golden Arduino V2, Powered Up**
![Top view powered up](images/05-top-view-powered.jpg)

**Fig. 6: Bottom View, Instrument Droid**
![Bottom view powered](images/06-bottom-view-powered.jpg)

## Summary of Board Design

My original Golden Arduino didn't perform as well as I'd hoped, so I wanted to recreate it. This time I reduced the board size and shortened trace distances to important components. I also opted for a 4-layer board — partly to stack Golden Arduino V2 on top of Instrument Droid, and partly to see how different the design process would be with more layers. Designing Golden Arduino V2 was a bit easier with 4 layers, though wiring up the ATmega was still a challenge given the board size. I reused most of the schematic from Board 3, but opted out of the 3.3V LDO this time. Instrument Droid's design was more straightforward — I gave myself space on the bottom layer for components, and for fun, added three smart LEDs and header pins for an OLED screen I plan to add later.

Getting this board working correctly was a real challenge. Soldering some components — the ATmega, the TVS, and the ADS — was difficult on its own. After soldering everything and successfully bootloading the ATmega, I ran into the main issue: the Arduino IDE recognized the board, but the sketch would upload infinitely, throwing `avrdude: stk500_recv(): programmer is not responding`.

At first I suspected the CH340G and TVS weren't working correctly. I replaced both — issue persisted. After a few hours of debugging, a TA and I found it: my ATmega's TXD and RXD pins were getting about 3.3V instead of the full 5V.

**Fig. 7: 5V power trace missing**
![Missing 5V power trace](images/07-missing-trace.png)

During the design phase, I'd added several 5V power traces assuming they'd all connect to a common 5V net — they didn't. One simple missing trace would have tied all the 5V power routing together and saved hours of debugging.

**Fig. 8: 5V power trace fix**
![5V power trace fix](images/08-trace-fix.png)

To fix it, I added a simple wire connecting all the 5V nets together. After that, all power traces got the full 5V they needed, and the infinite-upload issue was resolved. It's remarkable that one missing trace caused this much trouble — but with the fix in place, both Golden Arduino V2 and Instrument Droid worked as intended.

## Analysis of Instrument Droid

With the board issue resolved, I moved on to analyzing Instrument Droid.

**Fig. 9: IDE Output**
![IDE output](images/09-ide-output.png)

The ATmega328 is bootloaded and receiving commands from the Arduino IDE. To confirm everything was working, I probed the MOSFET and DAC to check they were turning on.

**Fig. 10: DAC and MOSFET Output**
![DAC and MOSFET output](images/10-dac-mosfet-output.png)

Both the MOSFET and DAC showed the same output, turning on and off and increasing with voltage as expected.

With Instrument Droid confirmed working, I collected data from three voltage sources: a 5V wall jack, a DC power supply at 9V, and a function generator at 5V.

**Fig. 11–12: 5V Wall Jack data and plot**
![5V jack data](images/11-5v-jack-data.png)
![5V jack plot](images/12-5v-jack-plot.png)

**Fig. 13–14: DC Power Supply (9V) data and plot**
![DC supply data](images/13-dc-supply-data.png)
![DC supply plot](images/14-dc-supply-plot.png)

**Fig. 15–16: Function Generator (5V) data and plot**
![Function generator data](images/15-funcgen-data.png)
![Function generator plot](images/16-funcgen-plot.png)

I picked the function generator specifically because it has a known Thevenin resistance of 50Ω, which let me validate Instrument Droid's accuracy directly. Instrument Droid calculated the source's Thevenin resistance at **around 48Ω** — close to the known 50Ω value, confirming Instrument Droid can accurately determine the Thevenin resistance of a voltage source.

**Fig. 17–19: Smart LEDs**
![Smart LEDs](images/17-smart-leds.png)

Added three smart LEDs for fun — an easy, programmable addition. Next step is adding an OLED screen (headers already placed) so results can be displayed directly instead of through the IDE.

## Conclusion

I'm very happy with how this board turned out. I wanted to challenge myself to recreate Board 3 while adding Instrument Droid at the same time, and it was an improvement over the last board. I plan to keep using 4-layer boards going forward. It took a lot of time to debug why the board wasn't uploading code — the fix turned out to be simple and avoidable with a more careful layout review.

Two main takeaways:

1. **Review your design before generating Gerbers.** More careful review of the layout would have caught the missing 5V trace before fab, avoiding a major functional issue.
2. **More test points save debugging time.** I should have added more test points throughout this board — it would have narrowed down the power issue faster, and taught me what to look for when specific subsystems aren't behaving.

Designing, building, and debugging this board pulled together everything I learned across the whole class. I'll be carrying this experience into personal projects and, hopefully, into my career.

---

Previous: [03 — Golden Arduino](../03-golden-arduino/)
Back to: [PCB Design & Manufacturing Portfolio](../)
