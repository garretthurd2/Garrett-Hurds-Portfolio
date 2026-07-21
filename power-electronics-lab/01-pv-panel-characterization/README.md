# PV Panel Characterization

**Course:** ECEN 4517 – Power Electronics Laboratory, University of Colorado Boulder
**Team:** Garrett Hurd, Christian Lopez

## Overview

Characterized an 85 W PV panel's I-V behavior under full sun and partial shading, then built and validated a single-diode equivalent circuit model in LTSpice. The measured data and derived converter specs from this stage fed directly into the buck converter / MPPT design in [Stage 02](../02-buck-converter-mppt).

## What Was Done

- Measured the panel's full I-V curve under unshaded and shaded (4 cells covered) conditions using a variable resistive load
- Fit a single-diode equivalent circuit model (Rs, Rp, saturation current, ideality factor) to the measured data
- Validated the model in LTSpice against both shaded and unshaded measurements, including a backplane-diode bypass model for the shaded case
- Directly charged a 12 V lead-acid battery from the panel to benchmark against MPPT tracking (used later in Stage 02)
- Derived preliminary DC-DC converter specs (input/output voltage and current ranges, switching frequency) to carry into the buck converter design

## Key Results

| Metric | Result |
|---|---|
| Unshaded max power point | 73.95 W @ 14.5 V, 5.1 A (Voc 21.1 V, Isc 5.4 A) |
| Shaded max power point (4 cells covered) | 32.16 W @ 6.7 V, 4.8 A — a ~56% power loss from shading |
| LTSpice model vs. measured (unshaded) | 74.6 W simulated vs. 73.95 W measured — within ~1% |
| LTSpice model vs. measured (shaded, w/ backplane diode) | 33.35 W simulated vs. 32.16 W measured — within ~4% |
| Direct battery charging (no MPPT) | 64 W — 86.6% of the panel's measured max power point |

## Figures

![Unshaded and shaded I-V curves](images/01-unshaded-iv-curve.png)
*Measured I-V curve, unshaded panel.*

![LTSpice equivalent circuit model validation](images/05-ltspice-iv-pv-curves.png)
*LTSpice single-diode model I-V and P-V curves vs. measured data.*

## Full Report

Full lab report (PDF) to be added to this folder.
