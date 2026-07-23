# Ohm++ — Adaptive Fishing Rod Controller

**Senior Capstone, Electrical & Computer Engineering, University of Colorado Boulder**

**Team:** Garrett Hurd (Project Lead, Power), Tyler Martin (Analog Circuits, Testing, RF), Pedro Mejido (System Architect, PCB), Vedant Shah (Software), Felipe Tamayo-Tapia (Embedded), Travis Wood (User Interface, Mechanical)

**Mentor:** Prof. Eric Bogatin, John Lettang

**Sponsors:** QL Plus (Scott Huyvaert), River Deep Foundation (Bob Adwar)

## Mission

A fishing rod controller for quadriplegic, paraplegic, and traumatic brain injury patients — built for recreational fishing outings at Craig Hospital, hosted by the River Deep Foundation. A single lever-and-button controller drives the casting, reeling, and bait-release motions of a modified commercial fishing rod, letting a user with limited hand dexterity fish independently. Every design decision was filtered through one question: does this help the end user actually use the device, not just work on a bench.

## System Architecture

<img width="742" height="532" alt="Block Diagram 3 2" src="https://github.com/user-attachments/assets/a4275069-06df-411e-9a75-78ed54cf3d9c" />

<img width="2556" height="2251" alt="IMG_0733" src="https://github.com/user-attachments/assets/b315be9d-4dcc-49a3-a90c-8a72b9b29fd9" />

<img width="2537" height="2357" alt="IMG_0734" src="https://github.com/user-attachments/assets/eec62414-6d5f-4dc5-bb7f-e601deacc78f" />

Two stacked PCBs: a bottom board carrying the power distribution, LDO, and all three motor driver circuits, and a top board carrying a custom Arduino-equivalent microcontroller plus the buck converter. 

## My Role: Power System

As Power lead, I owned the battery selection, voltage regulation, and motor driver circuits, plus backup System Architect / Analog Circuits support.

**Power distribution:** A 24V 12 Ah lithium drill battery (chosen so end users and recreational therapists could swap it like any cordless tool battery) feeds a buck converter down to 12V for the three motor drivers, then an LDO down to 5V for the microcontroller and controller electronics.

**Motor drivers:**
- **Reel motor:** a simple, effective low-side MOSFET + PWM design
- **Stepper motor (casting):** drives the casting arm, tensioning a spring for the back-cast and free-wheeling to release it forward
- **Thumb/linear actuator:** releases the bait-caster's clutch at a precisely timed point in the forward-cast arc (45°), timed via slow-motion camera analysis of the release point
- **H-bridge:** went through a full redesign mid-project. The original gate-driver approach (pulling gate voltage above drain voltage) was unfamiliar territory and cost about a month of debugging before we cut losses and switched to a simpler opamp-based logic-control design. Schottky diodes were added in parallel with the MOSFETs to dissipate back-EMF from the stepper's return spring; without them, the rod's return motion stuttered instead of moving smoothly.

**Buck converter — what didn't work:** the custom buck converter was one of the highest-risk parts of the design, and it failed twice. First attempt missed a feedback loop entirely. Second attempt (an IC-based design with integrated feedback) tested fine standalone but melted on contact with the actual system power rails during acceptance testing. Both PCB revisions kept header-pin compatibility with an off-the-shelf buck converter as a fallback specifically because this was flagged as high-risk early.

## Key Results

| Requirement | Target | Result |
|---|---|---|
| Battery life | 2 hours | 8 hours of continuous casting/reeling at Expo, battery still at ~75% charge |
| Safety | No exposed wiring, no excess heat/shock risk | Met — minimized current draw, sealed electrical housing |
| Weatherproofing | Rain-resistant | ABS enclosures, acetone vapor-sealed, coated in spray-on truck bed liner |
| Usability | New user operational in <5 minutes, no instruction beyond controller labels | Met. Validated by handing the controller to passersby |
| Cast release timing | Consistent 45° release angle | Achieved via iterative slow-motion camera calibration |

Current budget (calculated pre-build, confirmed against oscilloscope current measurements): casting motor 2.4 A during a 12-second max back-cast, reeling motor 0.41 A, thumb actuator 0.195 A — totaling roughly 1.26 Ah for a worst-case 2-hour trip at 30 casts/hour. The 12 Ah battery gave enormous headroom over that calculated minimum, which is exactly why it ran 8 hours instead of 2.

## What Didn't Work / Root Cause

- **Custom buck converter (×2):** missing feedback loop on the first pass; the second, IC-based attempt melted under real system load despite testing fine in isolation. A reminder that bench validation with a lab supply doesn't always catch what happens under the actual system's transient loads
- **H-bridge gate driver:** an unfamiliar gate-driver topology ate roughly a month before the team pivoted to a design everyone was confident in
- **Integration surprises:** individual circuits all worked independently, but stuffing everything into the final housing before acceptance testing surfaced new heating and solder-joint issues that hadn't shown up on the open bench

## Lessons Learned

- Iterative design and the willingness to cut losses on a failing approach (twice, on the buck converter; once, on the gate driver) mattered more than getting it right the first time
- Flagging high-risk subsystems early and building in a fallback path (the swappable buck converter board) is what kept the project on schedule when that risk materialized
- Hardware/software integration failures are often the smallest details — a misplaced variable, a single unconnected wire — and methodical, patient debugging is a learnable skill, not just a personality trait

## Images

<img width="3297" height="3308" alt="image" src="https://github.com/user-attachments/assets/1a085b02-0e2c-4bc0-aa88-f783075aebdc" />

*The Ohm++ team. Tyler Martin, Felipe Tamayo-Tapia, Vedant Shah, Garrett Hurd, Pedro Mejido, Travis Wood (left to right)*

<img width="4112" height="3761" alt="controller" src="https://github.com/user-attachments/assets/6cd10841-9927-4ca9-a413-26baefcf5332" />

*The final controller — lever, cast/reel mode buttons, and emergency stop, coated in truck bed liner for grip and weather resistance.*

<img width="3090" height="5371" alt="IMG_0741" src="https://github.com/user-attachments/assets/12a374e9-51b0-4ba5-87ff-2501d7f03d91" />

*Completed final device w/ new power system and controller.*

## Documents

Full capstone final reports, Critical Design Review presentation, and Preliminary Design Review presentation are included in this folder for viewing.
