# Power Electronics Lab

**Course:** ECEN 4517 – Power Electronics Laboratory, University of Colorado Boulder

**Team:** Garrett Hurd, Christian Lopez

A four-stage power system built from the ground up: characterizing a PV panel, converting and tracking its peak power into a battery, boosting that battery voltage to a high-voltage DC bus, and inverting that bus to household-outlet-equivalent AC. Each stage below is a standalone write-up, feel free to click through for the full breakdown, key results, and figures. Full lab report PDFs are included in each stage folder.

| Stage | What it is | Key result |
|---|---|---|
| [01 — PV Panel Characterization](01-pv-panel-characterization) | I-V characterization of an 85 W panel under shaded and unshaded conditions, modeled as a validated single-diode equivalent circuit in LTSpice | LTSpice model matched measured unshaded power within ~1% |
| [02 — Buck Converter & MPPT](02-buck-converter-mppt) | 100 kHz buck converter with battery current/voltage sensing and a duty-cycle-sweep MPPT algorithm, field-tested with the PV panel | 91% converter efficiency; MPPT tracked peak power at up to 97.4% efficiency in the field |
| [03 — Cascaded Boost Converter](03-cascaded-boost-converter) | Two-stage (cascaded) boost converter with a discrete analog PWM gate driver, stepping ~12 V to 150+ V | 76% efficiency at 150 V, full load |
| [04 — DC-AC Inverter](04-dc-ac-inverter) | Modified sine-wave inverter converting the 150 V DC bus to household-outlet-equivalent AC | 119.6 Vrms at 60.28 Hz |

## System Overview
(images/System Overview.png)
Each stage was designed, built, and bench-tested independently, then integrated and field-tested as a complete system outdoors with the PV panel and battery.
