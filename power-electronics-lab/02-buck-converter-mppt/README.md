# 02 — Buck Converter & MPPT

*ECEN 4517 — Christian Lopez & Garrett Hurd*

A buck converter stepping the PV panel's output down to charge a 12V lead-acid battery, with onboard current/voltage sensing and a maximum power point tracking (MPPT) algorithm running the duty cycle sweep in real time.

## Buck Converter Design

### Inductor Design

![Inductor design calculations](images/01-inductor-design-calcs.png)

Designed for 100 kHz switching, 24.2 µH of inductance, using a PQ 26/25 core with N95 ferrite. This called for 6 turns at a minimum wire gauge of 12 AWG.

| | Value |
|---|---|
| Measured inductance | 21.594 µH |
| Air gap used | 0.0075 in |
| Turns | 6 |
| Wire gauge | 18 |
| Measured DC resistance (Rs) | 8.8 mΩ |

The measured inductance came in close to the calculated value.

### Power Stage Construction

![Buck power stage, top view](images/02-buck-stage-top.png)
![Buck power stage, bottom view](images/03-buck-stage-bottom.png)

## Buck Converter Testing

### Nominal Load Testing

Tested at a load resistance modeling the lead-acid battery being charged. Target output was 85W at 13V (6.54A). At an 80% duty cycle, the converter delivered:

| | Voltage | Current | Power |
|---|---|---|---|
| Input | 17.2 V | 5.5 A | 94.6 W |
| Output | 13 V | 6.6 A | 85.8 W |

**Efficiency: 91%**, at a load resistance of 2.4Ω.

![Diode voltage — 10V, 4µs/div](images/04-diode-voltage.png)
![Transistor current — 5A, 4µs/div](images/05-transistor-current.png)
![Diode current — 5A, 4µs/div](images/06-diode-current.png)
![Inductor current — 1A, 4µs/div](images/07-inductor-current.png)

Measured max inductor current was 7.16A with a 6.68A mean, giving a 960mA ripple current. This was calculated pre-build at a 6.55A average with 1.31A (20%) ripple — the built converter showed a 20% higher average current and a 26% lower ripple than predicted, but the converter still regulated output voltage correctly across testing.

![Capacitor C1 current — 2A, 4µs/div](images/08-c1-current.png)
![Capacitor C1 voltage — 10V, 4µs/div](images/09-c1-voltage.png)

### Capacitor ESR & Power Loss

![C1 ESR voltage measurement — 500mV, 1µs/div](images/10-c1-esr-voltage.png)
![C1 ESR current measurement — 2A, 1µs/div](images/11-c1-esr-current.png)

ESR = ΔV/ΔI = 40mV / 3.44A = **11.6 mΩ**, versus the capacitor's datasheet value of 13 mΩ — a reasonable match. The ringing visible in these captures comes from wire inductance on the prototype board; shorter leads would reduce it.

Using the datasheet Irms rating, estimated power loss was 206.6 mW. Using the Irms actually measured in lab (1.32A), the real power loss works out closer to **20.2 mW** — comfortably in the milliwatt range.

### Load Variation Testing

**15W target** (11.4Ω load, 73.3% duty cycle): 15.82W in, 15.08W out → **95.3% efficiency**.

![Transistor current, 15W test](images/12-transistor-current-15w.png)
![Diode voltage, 15W test](images/13-diode-voltage-15w.png)

**1W target** (165Ω load, 23% duty cycle): 2.06W in, 1.01W out → **48.9% efficiency**. The efficiency drop at low duty cycle is expected — buck converters lose more proportionally at low duty cycles and shouldn't be run this low in practice.

![Transistor current, 1W test](images/14-transistor-current-1w.png)
![Diode voltage, 1W test](images/15-diode-voltage-1w.png)

## Battery Current & Voltage Sensing

### Sensing Circuit Design

![Sensing circuit schematic and calculations](images/16-sensing-schematic.png)

Rs1 and Rs2 were chosen in the kΩ range to minimize current draw through the sensing path while preserving usable voltage levels. A voltage divider steps the 15V maximum down to a 3.3V ADC-safe range. Rf1 and Rf2 (both 1kΩ) sit in series with the current sensor at a 1:1 ratio. All component values were selected to match what was available in the lab kit.

### Sensing Circuit Construction

![Sensing circuitry, top view](images/17-sensing-top.png)
![Sensing circuitry, bottom view](images/18-sensing-bottom.png)

### Sensing Accuracy Validation

![Measured vs. sensed output current vs. input voltage](images/19-current-accuracy.png)
![Measured vs. sensed output voltage vs. input voltage](images/20-voltage-accuracy.png)

Sensing circuitry tracked multimeter-measured values closely across a swept input voltage range. **Voltage sensing averaged 0.8% error; current sensing averaged 0.68% error** — accurate enough to drive the MPPT algorithm.

## MPPT Algorithm

A duty-cycle sweep algorithm implemented inside the ADC interrupt service routine, broken into three stages: variable initialization, initial measurement, and maximum-power-point setting.

![MPPT variable declarations](images/21-mppt-variables.png)
![Initial ADC measurement code](images/22-mppt-init-measurements.png)

If the battery voltage sits above 12.6V (a healthy state of charge), the buck converter stays off. Otherwise, duty cycle is swept from 55% to 95% in 2.5% steps; at each step the code waits for steady state, then averages 300 ADC samples of current and voltage.

![MPP setting code](images/23-mppt-setting-code.png)

At high duty cycles, sensed voltage/current were adjusted down 1% based on the accuracy data above, which had shown the sensed values running slightly high at high input voltages. Power is calculated at each duty step and compared against the running maximum; once the sweep finishes, the best duty cycle is held for 5 minutes while the system sleeps, with a voltage check every minute to prevent overcharging. If the battery hits 12.8V or the 5 minutes elapse, the sweep restarts.

### Benchtop Testing

Bench setup used two power supplies, a rheostat set to 3.4Ω (simulating the PV panel's input resistance), and the battery. Input voltage was set to 38V to land at the target 17.2V converter input through the resulting voltage divider.

| Cycle | Input V | Input I | Output V | Output I | Input P | Output P | Efficiency | Source |
|---|---|---|---|---|---|---|---|---|
| 1 | 17.0 | 4.7 | 13.7 | 4.4 | 79.9 | 60.3 | 75.4% | Multimeter |
| 2 | 17.1 | 4.7 | 14.0 | 5.3 | 80.4 | 74.2 | 92.3% | Multimeter |
| 3 | 17.2 | 4.7 | 14.1 | 5.3 | 80.8 | 74.7 | 92.4% | Multimeter |
| 4 | 16.7 | 4.8 | 14.1 | 5.3 | 80.2 | 74.7 | 93.2% | Multimeter |
| 5 | 17.5 | 4.5 | 14.0 | 5.3 | 78.8 | 74.2 | 94.2% | Multimeter |
| 6 | 17.1 | 4.7 | 14.0 | 5.3 | 80.4 | 74.2 | 92.3% | Multimeter |
| 7 | 17.1 | 4.7 | 14.1 | 5.3 | 80.4 | 74.7 | 93.0% | Multimeter |
| 8 | 17.1 | 4.7 | 14.1 | 5.3 | 80.4 | 74.7 | 93.0% | Multimeter |
| 1 | 17.1 | 4.7 | 14.2 | 6.1 | 78.7 | 86.6 | 107.5% | Computer (sensed) |
| 2 | 17.1 | 4.8 | 14.1 | 5.6 | 78.7 | 79.5 | 101.1% | Computer (sensed) |
| 3 | 17.2 | 4.6 | 14.2 | 5.6 | 79.1 | 79.6 | 100.6% | Computer (sensed) |
| 4 | 17.1 | 4.7 | 14.2 | 5.6 | 80.4 | 79.5 | 98.9% | Computer (sensed) |
| 5 | 17.1 | 4.7 | 14.3 | 5.5 | 80.4 | 78.7 | 97.9% | Computer (sensed) |
| 6 | 17.2 | 4.7 | 14.2 | 5.5 | 80.8 | 78.7 | 97.4% | Computer (sensed) |
| 7 | 17.4 | 4.6 | 14.1 | 6.1 | 80.0 | 78.7 | 98.3% | Computer (sensed) |
| 8 | 17.0 | 4.7 | 13.9 | 5.6 | 79.9 | 78.7 | 98.5% | Computer (sensed) |

The sensed (computer) data repeatedly found the maximum power point — while the absolute power readings were off by about 5W from the multimeter reference, the algorithm reliably detected increases and decreases in output power. **The converter consistently found 74.7W of maximum power at 93% efficiency.**

![Maximum power point tracking over time](images/24-mppt-tracking.png)

The algorithm found a local maximum around 72W early in the sweep, then continued finding higher power as duty cycle increased, ultimately landing on the true maximum at a 95% duty cycle.

![MPPT charge-control turn-off feature](images/25-mppt-turnoff.png)

The charge-control feature shuts the converter off above 12.8V and re-enables it below 12.6V. The system turns off at the 450-second mark in this capture — the turnoff check runs at the end of the program plus a few delays, which is why the actual voltage crossing shows over two minutes late.

![Filtered Vpwm and Vin](images/26-filtered-vpwm.png)

A 150Ω + 100nF low-pass filter (10kHz corner) was used to view the PWM signal. Noticeable noise appears at the switching transitions, most likely from the long prototype wiring being susceptible to the high di/dt of the switching edges.

## Field Testing

### Unshaded Solar Testing

Tested outdoors at 1:39 PM, solar irradiance measured at 1160 W/m².

| Vin | Iin | Pin | Vout | Iout | Pout | Efficiency | Duty Cycle |
|---|---|---|---|---|---|---|---|
| 14.805 | 4.840 | 71.656 | 13.705 | 5.100 | 69.896 | 97.5% | 95 |
| 14.540 | 4.590 | 66.739 | 13.100 | 4.950 | 64.845 | 97.2% | 95 |
| 14.801 | 4.807 | 71.148 | 13.699 | 5.047 | 69.139 | 97.2% | 95 |
| 14.212 | 4.747 | 67.464 | 13.526 | 4.879 | 65.993 | 97.8% | 95 |
| 14.693 | 4.760 | 69.939 | 13.604 | 5.006 | 68.102 | 97.4% | 95 |
| 14.691 | 4.748 | 69.753 | 13.597 | 5.001 | 67.999 | 97.5% | 96 |
| 14.729 | 4.771 | 70.272 | 13.629 | 5.012 | 68.309 | 97.2% | 96 |
| 14.575 | 4.665 | 67.992 | 13.145 | 5.045 | 66.317 | 97.5% | 92 |

The battery started this test at a below-50%-SOC 12.04V (left that way by the previous group), so the turn-off charging feature couldn't be exercised. Even so, the system performed well: **average max power point of 67.57W at 69.37W input, a 97.4% average efficiency.** Absolute power was lower than the panel's rating, but the high efficiency was a good sign the system itself was working correctly.

![Low-pass filtered Vpwm and Vpv, unshaded field test](images/27-unshaded-vpwm-vpv.png)

Compared against Experiment 2's purely-resistive-load characterization (73.95W max), the MPPT system landed within 3.2% of that reference max — reasonable, given the different buck converter, load, sensing circuitry, and starting battery voltage between experiments. The MPPT system also outperformed simple direct energy transfer to the battery by 5.6%, confirming the converter added real value over just wiring the panel straight to the battery.

### Shaded Solar Testing

Tested with 4 panel cells covered by a thick coat, at 3:03 PM, solar irradiance 1120 W/m².

| Vin | Iin | Pin | Vout | Iout | Pout | Efficiency | Duty Cycle |
|---|---|---|---|---|---|---|---|
| 12.461 | 0.254 | 3.16 | 12.127 | 0.262 | 3.17 | 100.4% | 96 |
| 12.425 | 0.217 | 2.69 | 12.099 | 0.223 | 2.70 | 100.2% | 96 |
| 12.344 | 0.124 | 1.53 | 12.088 | 0.133 | 1.61 | 105.1% | 95 |
| 12.057 | 0.644 | 7.76 | 12.200 | 0.676 | 8.25 | 106.2% | 95 |
| 13.970 | 0.614 | 8.58 | 12.180 | 0.700 | 8.53 | 99.4% | 87 |
| 12.850 | 0.681 | 8.75 | 12.190 | 0.716 | 8.73 | 99.7% | 95 |

![Vpwm and Vpv waveforms, shaded field test](images/28-shaded-vpwm-vpv.png)

Unlike the unshaded results, this data was noticeably more sporadic — average output power of only 5.5W at an average "efficiency" of 101.8% (a physically impossible number that flags something in the system wasn't behaving correctly, most likely the low starting battery SOC combined with panel shading). At lower PV voltages there was visibly less switching ringing, and the filtered PWM signal took on a more triangular shape resembling capacitor charge/discharge behavior rather than the sharper waveform seen unshaded.

Compared against Experiment 2's shaded characterization (32.16W max at the panel), the MPPT system only found a maximum input power of **8.75W** — dramatically lower than expected, and roughly 3x below even the ~27.8W a simple direct-energy-transfer estimate would predict for this shading level.

## Open Questions

The shaded-condition discrepancy was never fully resolved during testing. Possible causes identified: the battery's low state of charge pulling more current at a lower voltage than the algorithm expected, an error in how the MPPT algorithm responds under shaded/low-power conditions, or measurement technique during that specific test run. This would need dedicated follow-up testing — specifically at low input voltage and current — to isolate the actual cause.

Two directions were identified for improving shaded performance: a buck-boost converter stage instead of buck-only, to better extract available power under shading, or a more adaptive MPPT algorithm tuned differently for shaded versus unshaded conditions.

This stage feeds the battery bus forward into the [Cascaded Boost Converter](../03-cascaded-boost-converter/).
