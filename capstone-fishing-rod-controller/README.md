# Ohm++ — Adaptive Fishing Rod Controller

**Senior Capstone, Electrical & Computer Engineering, University of Colorado Boulder**

**Team:** Garrett Hurd (Project Lead, Power), Tyler Martin (Analog Circuits, Testing, RF), Pedro Mejido (System Architect, PCB), Vedant Shah (Software), Felipe Tamayo-Tapia (Embedded), Travis Wood (User Interface, Mechanical)

**Mentor:** Prof. Eric Bogatin, John Lettang

**Sponsors:** QL Plus (Scott Huyvaert), River Deep Foundation (Bob Adwar)

## Mission

A fishing rod controller for quadriplegic, paraplegic, and traumatic brain injury patients — built for recreational fishing outings at Craig Hospital, hosted by the River Deep Foundation. A single lever-and-button controller drives the casting, reeling, and bait-release motions of a modified commercial fishing rod, letting a user with limited hand dexterity fish independently. Every design decision was filtered through one question: does this help the end user actually use the device, not just work on a bench.

## System Architecture

<img width="741" height="531" alt="Block_Diagram_V3 1drawio1" src="https://github.com/user-attachments/assets/d26367ad-80c5-4b61-b2af-52e34d60144c" />

<img width="2556" height="2251" alt="IMG_0733" src="https://github.com/user-attachments/assets/b315be9d-4dcc-49a3-a90c-8a72b9b29fd9" />

<img width="2537" height="2357" alt="IMG_0734" src="https://github.com/user-attachments/assets/eec62414-6d5f-4dc5-bb7f-e601deacc78f" />

Two stacked PCBs: a bottom board carrying the power distribution, LDO, and all three motor driver circuits, and a top board carrying a custom Arduino-equivalent microcontroller plus the buck converter — built in two swappable versions (custom and commercial buck converter) so a failure in one wouldn't take down the whole system before Expo.

## My Role: Power System

As Power lead, I owned the battery selection, voltage regulation, and motor driver circuits, plus backup System Architect / Analog Circuits support.

**Power distribution:** A 24V 12 Ah lithium drill battery (chosen so end users and recreational therapists could swap it like any cordless tool battery) feeds a buck converter down to 12V for the three motor drivers, then an LDO down to 5V for the microcontroller and controller electronics.

**Motor drivers:**
- **Reel motor:** a simple, effective low-side MOSFET + PWM design — one of the few subsystems that worked essentially on the first try
- **Stepper motor (casting):** drives the casting arm, tensioning a spring for the back-cast and free-wheeling to release it forward
- **Thumb/linear actuator:** releases the bait-caster's clutch at a precisely timed point in the forward-cast arc (45°), timed via slow-motion camera analysis of the release point
- **H-bridge:** went through a full redesign mid-project — the original gate-driver approach (pulling gate voltage above drain voltage) was unfamiliar territory and cost about a month of debugging before we cut losses and switched to a simpler opamp-based logic-control design. Schottky diodes were added in parallel with the MOSFETs to dissipate back-EMF from the stepper's return spring; without them, the rod's return motion stuttered instead of moving smoothly.

**Buck converter — what didn't work:** the custom buck converter was one of the highest-risk parts of the design, and it failed twice. First attempt missed a feedback loop entirely. Second attempt (an IC-based design with integrated feedback) tested fine standalone but melted on contact with the actual system power rails during acceptance testing. Both PCB revisions kept header-pin compatibility with an off-the-shelf buck converter as a fallback specifically because this was flagged as high-risk early — that fallback is what shipped at Expo.

## Key Results

| Requirement | Target | Result |
|---|---|---|
| Battery life | 2 hours | 8 hours of continuous casting/reeling at Expo, battery still at ~75% charge |
| Safety | No exposed wiring, no excess heat/shock risk | Met — minimized current draw, sealed electrical housing |
| Weatherproofing | Rain-resistant | ABS enclosures, acetone vapor-sealed, coated in spray-on truck bed liner |
| Usability | New user operational in <5 minutes, no instruction beyond controller labels | Met — validated by handing the controller to passersby |
| Cast release timing | Consistent 45° release angle | Achieved via iterative slow-motion camera calibration |

Current budget (calculated pre-build, confirmed against oscilloscope current measurements): casting motor 2.4 A during a 12-second max back-cast, reeling motor 0.41 A, thumb actuator 0.195 A — totaling roughly 1.26 Ah for a worst-case 2-hour trip at 30 casts/hour. The 12 Ah battery gave enormous headroom over that calculated minimum, which is exactly why it ran 8 hours instead of 2.

## What Didn't Work / Root Cause

- **Custom buck converter (×2):** missing feedback loop on the first pass; the second, IC-based attempt melted under real system load despite testing fine in isolation — a reminder that bench validation with a lab supply doesn't always catch what happens under the actual system's transient loads
- **H-bridge gate driver:** an unfamiliar gate-driver topology ate roughly a month before the team pivoted to a design everyone was confident in
- **Integration surprises:** individual circuits all worked independently, but stuffing everything into the final housing before acceptance testing surfaced new heating and solder-joint issues that hadn't shown up on the open bench

## Lessons Learned

- Iterative design and the willingness to cut losses on a failing approach (twice, on the buck converter; once, on the gate driver) mattered more than getting it right the first time
- Flagging high-risk subsystems early and building in a fallback path (the swappable buck converter board) is what kept the project on schedule when that risk materialized
- Hardware/software integration failures are often the smallest details — a misplaced variable, a single unconnected wire — and methodical, patient debugging is a learnable skill, not just a personality trait

## Figures

![Team at Capstone Expo with the finished device](images/01-team-expo-photo.png)
*The Ohm++ team at Capstone Expo with the completed adaptive fishing rod controller.*

![Bottom PCB — power distribution and motor drivers](images/02-bottom-power-distribution-pcb.png)
*Bottom board: power distribution, LDO, and reel/stepper/thumb motor driver circuits.*

![Handheld controller](images/03-controller-photo.png)
*The final controller — lever, cast/reel mode buttons, and emergency stop, coated in truck bed liner for grip and weather resistance.*

![Current consumption analysis](images/04-current-consumption-analysis.png)
*Pre-build current budget analysis for the casting motor, confirmed against oscilloscope current measurements.*

## Full Report

Full capstone final report (PDF), including all Altium schematics/layouts, team retrospectives, and additional figures, to be added to this folder.
