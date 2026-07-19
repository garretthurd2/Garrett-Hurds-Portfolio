# Power Electronics & Photovoltaic Lab

*ECEN 4517 — Christian Lopez & Garrett Hurd*

A complete solar power system built and tested stage by stage: PV panel → Buck MPPT converter → battery → Cascaded Boost converter → DC-AC inverter → AC load. Each stage was designed, simulated, built on a prototyping board, and validated on the bench and in the field.

![System block diagram](images/system-overview.png)

**Tools:** LTSpice · Tektronix oscilloscope · Bode 100 impedance analyzer · soldering iron and a lot of patience

| Stage | What it is | Key result |
|---|---|---|
| [01 — PV Panel Characterization](01-pv-panel-characterization/) | I-V curve characterization of the panel under shaded and unshaded conditions, modeled as an equivalent circuit in LTSpice | Model matched measured data to within 1% |
| [02 — Buck Converter & MPPT](02-buck-converter-mppt/) | Buck converter with battery current/voltage sensing and a duty-cycle-sweep MPPT algorithm | 91% converter efficiency; MPPT found peak power at up to 97% efficiency |
| [03 — Cascaded Boost Converter](03-cascaded-boost-converter/) | Two-stage boost converter driven by a discrete analog PWM gate driver, stepping the battery voltage to 150V+ | 76% efficiency at full load |
| [04 — DC-AC Inverter](04-dc-ac-inverter/) | Modified sine-wave inverter converting the high-voltage DC bus to household-outlet AC | 119.6 Vrms at 60.28 Hz |

## Highlight: Buck Converter & MPPT

The most complete closed-loop system in the lab: a buck converter with onboard current/voltage sensing (accurate to under 1% error) feeding a duty-cycle-sweep MPPT algorithm running in real time on a microcontroller. On the bench, the algorithm reliably found the true maximum power point at 93% efficiency. In unshaded outdoor field testing, it held **97.4% average efficiency**.

Shaded field testing was a different story — output power dropped to a fraction of what the panel's own characterization data predicted it should deliver, with an average "efficiency" reading of 101.8%, a physically impossible number that's really a flag that something in the system wasn't behaving as expected. That discrepancy was never fully resolved; the [stage write-up](02-buck-converter-mppt/) documents the investigation and the leading hypotheses.

## What Four Stages Taught Me

- **Loop inductance shows up as real losses.** Long prototype leads on both the buck and boost stages introduced measurable ringing on switching transitions — visible directly in scope captures, and a likely contributor to efficiency coming in under the target on both converters.
- **Sensing accuracy has to earn the control loop's trust.** Voltage/current sensing was accurate to under 1% error, but even that small a bias had to be corrected for at high duty cycles to keep the MPPT algorithm converging on the right answer.
- **Design constraints cascade between stages.** The gate driver's switching frequency was deliberately dropped from a 100kHz design target to 93kHz late in the process, specifically to unlock the higher duty cycle the boost converter needed to hit its output voltage target — a tradeoff made across, not within, a single stage.
- **Nominal-condition testing doesn't catch everything.** The MPPT algorithm performed well bench-tested and field-tested under normal sun — but shaded field conditions exposed a discrepancy that no amount of unshaded testing would have revealed.
- **An impossible number is still useful data.** A measured 101.8% efficiency can't be real, but treating it as a bug report rather than a fluke is what turns a broken test into a debugging lead.
