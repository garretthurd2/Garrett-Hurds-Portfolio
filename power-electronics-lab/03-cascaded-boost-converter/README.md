# 03 — Cascaded Boost Converter

*ECEN 4517 — Christian Lopez & Garrett Hurd*

A two-stage cascaded boost converter stepping the battery bus up to a high-voltage DC rail, driven by a discrete analog PWM gate driver circuit.

## Gate Driver Circuit Design

### Analog Pulse Width Modulator

![LTSpice schematic of PWM circuit](images/01-pwm-schematic.png)

Built around the SG3525A PWM IC, fed 12V and producing two PWM outputs. Duty cycle is set by a potentiometer on the IC's NI pin, feeding two generic gate driver stages (1.5A max output each) ahead of the actual driver ICs used in the lab kit.

### Construction & Testing of the Gate Driver Circuit

![Power signals — VIN, GND, VC, VRF](images/02-power-signals.png)
![Output PWM signal — OUA, OUB, Gate Driver 1, Gate Driver 2](images/03-output-pwm.png)
![Inverting, non-inverting, CT, and discharge pins](images/04-inv-ni-ct-dch.png)
![SHD, SS, CMP, and OSC pins](images/05-shd-ss-cmp-osc.png)
![RT and SYN pins](images/06-rt-syn.png)

Waveforms captured across all 16 pins of the PWM IC plus both gate driver outputs, with duty cycle set to its maximum of 88%. At this setting, the circuit produced its maximum duty cycle at **93 kHz**.

### Physical Circuitry vs. Simulation

![Simulation circuit diagram](images/07-sim-circuit.png)
![Simulation waveforms of the PWM circuit](images/08-sim-waveforms.png)

The physical circuit matched simulation in every respect except switching frequency — simulation was designed for 100 kHz, while the physical build ran at 93 kHz. This was a deliberate tradeoff: RT and RD resistor values were adjusted at the last minute to raise the maximum achievable duty cycle, which was necessary to reach the boost converter's target output voltage.

## Inductor Design

![L1 inductor value calculation](images/09-l1-calc.png)
![L2 inductor value calculation](images/10-l2-calc.png)
![L1 and L2 turns calculations](images/11-turns-calc.png)
![L1 and L2 AWG calculations](images/12-awg-calc.png)
![L1 and L2 air gap calculations](images/13-airgap-calc.png)

N95 ferrite was selected for both inductors for its lower core losses at 100 kHz-class switching frequencies, using a PQ 26/25 core. L1 was designed for 30.38 µH; L2 was calculated at 380 µH but built at 500 µH to target a 30% ripple on L2.

| | L1 | L2 |
|---|---|---|
| Calculated inductance | 30.38 µH | 380 µH (built at 500 µH) |
| Measured inductance (Bode 100) | 36 µH | ~360 µH |
| Measured DC resistance | 7.185 mΩ | 39.18 mΩ |

![Measured inductance values of L1 and L2](images/14-measured-inductance.png)

Measured values landed within margin of the design targets on both inductors.

## Cascaded Boost Power Stage Construction

![Power stage, top view](images/15-power-stage-top.png)
![Power stage, bottom view](images/16-power-stage-bottom.png)

## Cascaded Boost Converter Testing

### Current & Voltage Measurements

![Inductor 1 current](images/17-l1-current.png)

Inductor L1 current measured at the nominal operating point.

> ⚠️ **Check this against your original data:** the report text states an average current of "9 A" here, but the scope capture shows a Mean of 3.01A (Pk-Pk 2.88A), and the ripple calculation in this same section uses the 2.88A peak-to-peak value to get 1.44A ripple — consistent with the scope reading, not the "9 A" figure. Worth confirming which number is correct before this goes live.

Measured ripple of 1.44A compares to a calculated design ripple of 1.416A — a close match.

![Inductor 2 current](images/18-l2-current.png)

L2 measured an average current of 2.31A with a 0.86A ripple, against a calculated 2.0A average and 0.40A ripple. The gap between calculated and measured L2 values is larger than L1's, possibly attributable to the large wire loops used to make the current measurement.

![Q1 drain-to-source voltage](images/19-q1-vds.png)
![Q2 drain-to-source voltage](images/20-q2-vds.png)

A differential probe was used for both MOSFET drain-to-source measurements, even though both sources are grounded (which would technically allow single-ended measurement), to ensure accuracy.

| | Calculated | Measured |
|---|---|---|
| Q1 drain voltage | 42.43 V | 43.2 V |
| Q2 drain voltage | 150 V | 164 V |

Q2's measured "High" reading spiked as far as 216V, which is believed to be switching ringing rather than a true steady voltage — a product of the high di/dt through this stage combined with the large wire loops used for the current measurements.

### Converter Efficiency & Results

Producing the target 150V output at a 73% gate driver duty cycle:

| | Voltage | Current | Power |
|---|---|---|---|
| Input | 12.02 V | 9.04 A | 108.7 W |
| Output | 153.2 V | 0.54 A | 82.73 W |

**Efficiency: 76.12%**, against an expected ~80% average. The gap is believed to come from the same wiring issue showing up throughout testing — the inductor, MOSFET, and diode current loops were long enough to create measurable ringing and switching losses. Shortening those loops on a future revision should close most of the gap between measured and expected efficiency.

This stage's 150V+ DC bus feeds forward into the [DC-AC Inverter](../04-dc-ac-inverter/).

