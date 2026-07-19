# Power Electronics & Photovoltaic Lab

ECEN 4517 — Christian Lopez & Garrett Hurd

A complete solar power system built and tested stage by stage: PV panel → Buck MPPT converter → battery → Cascaded Boost converter → DC-AC inverter → AC load. Each stage was designed, simulated, built on a prototyping board, and validated on the bench with real load testing.

![System block diagram](images/system-overview.png)

## Stages

### [01 — PV Panel Characterization](01-pv-panel-characterization/)
Measured the panel's I-V curve under shaded and unshaded conditions and built an equivalent circuit model, validated in LTSpice to within 1% of measured data.

### [02 — Buck Converter & MPPT](02-buck-converter-mppt/)
Designed and built a buck converter with battery current/voltage sensing and a maximum power point tracking algorithm. **91% efficiency** at 85.8W, MPPT found peak power at up to **97% efficiency** in bench testing.

### [03 — Cascaded Boost Converter](03-cascaded-boost-converter/)
Two-stage boost converter driven by a discrete analog PWM gate driver, stepping the battery voltage up to 150V+ DC. **76% efficiency** at full load.

### [04 — DC-AC Inverter](04-dc-ac-inverter/)
Modified sine-wave inverter converting the high-voltage DC bus to a 60Hz AC output — 119.6 Vrms — representative of a standard household outlet.

## What I'd Do Differently

<!-- TODO: your honest note — e.g. the ringing/loop inductance issue that shows up in several of your reports (long leads on the prototyping boards causing switching noise), and the shaded MPPT discrepancy that needed more investigation. Real self-critique here is stronger than pretending everything was perfect. -->
