# 01 — PV Panel Characterization

*ECEN 4517 — Christian Lopez & Garrett Hurd*

## Nameplate Data

| Parameter | Value |
|---|---|
| Maximum Power | 85 W |
| Short Circuit Current | 5.45 A |
| Open Circuit Voltage | 22.2 V |
| Rated Current | 4.95 A |
| Rated Voltage | 17.2 V |
| Series Fuse | 20 A |
| Max System Open Circuit Voltage | 600/715 V |

The above table shows the PV panel's specifications as listed on the nameplate.

## Initial Daylight Measurements

| Measurement | Value |
|---|---|
| Pyranometer Value | 1072 |
| Time of Day | 1:35 PM |
| Battery Voltage | 12.99 V |
| Frequency | 61.7 Hz |
| With Lightbulb — Power | 26 W |
| With Lightbulb — Voltage | 115 V |
| With Lightbulb — Current | 0.226 A |

Initial measurements of the AC inverter, both with and without a load connected.

## Unshaded PV Panel Measurements

| Voltage (cell) | Voltage (Panel) | Current (A) | Power (W) |
|---|---|---|---|
| 0.542 | 19.5 | 1.5 | 29.25 |
| 0.539 | 19.4 | 1.6 | 31.04 |
| 0.536 | 19.3 | 1.8 | 34.74 |
| 0.525 | 18.9 | 2.3 | 43.47 |
| 0.511 | 18.4 | 2.9 | 53.36 |
| 0.486 | 17.5 | 3.6 | 63 |
| 0.475 | 17.1 | 4 | 68.4 |
| 0.464 | 16.7 | 4.4 | 73.48 |
| 0.403 | 14.5 | 5.1 | 73.95 |
| 0.306 | 11 | 5.3 | 58.3 |
| 0.114 | 4.1 | 5.3 | 21.73 |
| 0.197 | 7.1 | 5.3 | 37.63 |
| 0.094 | 3.4 | 5.3 | 18.02 |

All PV panel data collected to determine the maximum power point. After adjusting the rheostat, the maximum power point was determined to be 73.95 W with a Vmpp of 14.5 V at 5.1 A. These measurements were taken using two multimeters, one configured to read amperage and the other to read voltage, recorded at 1:47 PM.

## Characterizing the Unshaded PV Panel

![Unshaded PV I-V curve](images/01-unshaded-iv-curve.png)

The complete I-V curve of the PV panel. The measured short circuit current was 5.4 A and the measured open circuit voltage was 21.1 V. The maximum power point remained the same at 73.95 W at 14.5 V and 5.1 A. The curve takes the expected I-V curve shape for this device with few to no outliers.

## Characterizing the Shaded PV Panel

| Voltage (cell) | Voltage (panel) | Current (A) | Power (W) |
|---|---|---|---|
| 0.261 | 9.4 | 0.8 | 7.52 |
| 0.258 | 9.3 | 0.9 | 8.37 |
| 0.256 | 9.2 | 1.14 | 10.49 |
| 0.253 | 9.1 | 1.3 | 11.83 |
| 0.251 | 9.05 | 1.47 | 13.30 |
| 0.250 | 9 | 1.5 | 13.50 |
| 0.244 | 8.8 | 1.8 | 15.84 |
| 0.244 | 8.8 | 1.98 | 17.42 |
| 0.232 | 8.34 | 2.84 | 23.69 |
| 0.225 | 8.1 | 3.2 | 25.92 |
| 0.186 | 6.7 | 4.8 | 32.16 |
| 0.156 | 5.6 | 4.94 | 27.66 |
| 0.058 | 2.1 | 5.1 | 10.71 |
| 0.037 | 1.32 | 5.1 | 6.73 |

Data collected to determine the maximum power point with 4 cells of the PV panel shaded. The maximum power point was determined to be 32.16 W at 6.7 V and 4.8 A.

![Shaded PV I-V curve](images/02-shaded-iv-curve.png)

The short circuit current was measured at 5.3 A, and the open circuit voltage was measured at 9.8 V.

## Charging the Battery

| Parameter | Value |
|---|---|
| Battery Voltage | 12.8 V |
| Battery Current | 5.0 A |
| Power | 64 W |

Data collected while using the PV panel to charge the battery on the cart directly. The panel output 64 W, meaning it was not operating at its maximum power point — assuming the measured data reflects the true maximum, 64 W represents 86.6% of its total power potential.

## Efficiency Calculations

After visiting the NREL website, the direct normal insolation was estimated at 1100 W/m². Using this estimate, the panel's efficiency was calculated to be 10.63%.

Compared to the datasheet, this was lower than expected — the datasheet lists panel efficiency at an irradiance of 1000 W/m² as 13.4%. The discrepancy could be linked to outdoor temperature on the day this data was collected, though this isn't confirmed and would need further investigation to pin down the root cause.

## Equivalent Circuit Model

| Parameter | Value |
|---|---|
| Ki | 0.00515 |
| Ido | 820 × 10⁻¹² |
| Rs | 0.0114 |
| Rp | 1.54 |

Equivalent circuit model parameters found by the MATLAB model.

![I-V characteristics of the solar cell model vs. measured data](images/03-equivalent-circuit-iv.png)

The plot shows four curves: the modeled I-V curve (solid red), the experimentally measured data (dashed blue, partially hidden by the red line), the Voc/Vmpp/Impp/Isc reference points (solid blue), and the corresponding power curve (green).

The model fits the measured data well overall, though the open circuit voltage and short circuit current show some deviation — the model predicts an open circuit voltage of about 0.530 V, while the experimental data reflects 0.580 V.

## LTSpice Analysis of Circuit Model Parameters

| | Experiment 1 | Experiment 2 |
|---|---|---|
| Vmpp (V) | 17.2 | 14.5 |
| Impp (A) | 4.95 | 5.1 |
| Voc (V) | 22.2 | 21.1 |
| Isc (A) | 5.45 | 5.4 |
| Pmpp (W) | 85.14 | 73.9 |

Comparison between Experiment 1 and Experiment 2 maximum power point data. Experiment 2's Pmpp came in 13.3% lower than Experiment 1.

![LTSpice PV array model schematic](images/04-ltspice-schematic.png)

The LTSpice schematic used to verify the modeled equivalent circuit parameters.

![Simulated I-V curve](images/05-ltspice-iv-curve.png)

The simulation showed a Pmpp of 74.6 W — only about 1% off from what was experimentally measured. The I-V curve shows a Voc around 21 V, as expected for the 1000 W/m² irradiance level.

![Simulated P-V curve](images/06-ltspice-pv-curve.png)

The P-V curve shows a Pmpp of about 75 W at 16.4 V.

## PV Panel LTSpice Simulation with Backplane Diodes

![Shaded PV panel schematic with backplane diodes](images/07-backplane-diode-schematic.png)

Schematic of the shaded PV panel with two backplane diodes added.

![Simulated I-V curve, shaded panel](images/08-backplane-iv-curve.png)

This curve closely matches the curve built from the shaded data collected earlier in this report. The simulated open circuit voltage was 9.75 V versus 9.8 V measured experimentally. Simulated short circuit current was 5.01 A versus 5.3 A measured. The simulated maximum power point was 33.35 W versus 32.16 W measured, at a Vmpp of 7.4 V and Impp of 4.50 A. Of the two diodes, only the bottom diode (D2) conducts current in this simulation.

![Simulated P-V curve, shaded panel](images/09-backplane-pv-curve.png)

![Shaded I-V curve with Experiment 2 data overlaid](images/10-backplane-iv-overlay.png)

The experimental data points were overlaid on the simulated curve for comparison, using a third-party tool.

## Battery Characterization

| | 1:35 PM | 2:03 PM |
|---|---|---|
| Battery Voltage (V) | 12.99 | 12.56 |

Over 28 minutes, the battery lost 430 mV. Using the current draw measurement from the initial daylight test (0.226 A with the load lightbulb), this corresponds to 0.1107 Ah delivered by the battery, accounting for 95% inverter efficiency.

The ending battery voltage of 12.56 V corresponds to a state of charge of roughly 75%. State of charge follows a roughly linear relationship between 75% and 25%, but changes more dramatically outside that range — likely to keep the battery from dropping below 80% of its max charge.

| Battery Voltage (V) | Battery Current (A) | Battery Power (W) |
|---|---|---|
| 12.8 | 5 | 64 |

Measurements taken while using the PV panel to directly charge the battery.

Neglecting converter losses, an MPPT stage would be expected to increase the Pmpp from the measured 73.9 W to the panel's rated 85 W — a 14% power increase — while current would decrease about 4% and voltage would increase from the measured 14.5 V to the datasheet's 17.2 V, a 16% increase.

For shaded conditions, two directions for improvement were identified: a buck-boost converter to better extract available power under shading, or a more adaptive MPPT algorithm trained to handle shaded and unshaded conditions differently.

## Preliminary DC-DC Converter Specifications

- **Converter input:** 3.4 V to 19.4 V, 1.5 A to 5.3 A — experimentally gathered results of the PV panel's output.
- **Converter output:** 10.5 V to 13 V, 0 A to 5.3 A — experimentally gathered results of the battery's state-of-charge voltage and current.
- **Switching frequency:** 100 kHz — used in the prelab for Experiment 3 to design the inductor.
- **Efficiency target:** 70% at Vin = 17 V, Vout = 12 V, using the ideal buck converter conversion ratio M(D) = Vout/Vin.

These specifications carried directly into the [Buck Converter & MPPT](../02-buck-converter-mppt/) stage.
