# 04 — DC-AC Inverter

*ECEN 4517 — Christian Lopez & Garrett Hurd*

A modified sine-wave inverter converting the cascaded boost converter's high-voltage DC bus into an AC output representative of a standard household outlet.

## Circuit Design

![Modified sine-wave inverter circuit diagram](images/01-inverter-schematic.png)

Built as a half-bridge gate driver stage. The diagram shows the left (positive) side of the inverter; the right (negative) side driving Vac(t) is a mirror of the same circuit, otherwise identical. Key design values: CB = 10µF bootstrap cap, RH = 5Ω matched to a 5Ω load (RL), and RDT = 68kΩ tuned to give roughly 200ns of dead time between switching transitions. Gate control comes from a microcontroller over twisted-pair connections into the half-bridge driver IC.

## Circuit Construction

![Inverter circuit, top view](images/02-inverter-top.png)
![Inverter circuit, bottom view](images/03-inverter-bottom.png)

## Gate Control Signals

To produce the modified sine wave shape, the `half_duty` variable was set to 20,000 and the counter mode (`CTRMODE`) was changed to up-down counting — together these adjust the duty cycle and phase relationship of the two gate driver waveforms.

![Gate driver control code](images/04-gate-driver-code.png)

## Testing

![Output RMS voltage waveform](images/05-output-voltage-waveform.png)
![Output RMS current waveform](images/06-output-current-waveform.png)

The inverter produced **119.6 Vrms at 0.287 Arms**, running at **60.28 Hz** — matching the frequency of a standard household AC outlet.

![Gate driver output voltage waveform](images/07-gate-driver-output.png)

Gate driver duty cycle measured at approximately **68%**.

![Multimeter readings — DC input and AC output](images/08-multimeter-readings.png)

The top pair of multimeters captured the DC input voltage and current into the inverter stage; the bottom pair captured the AC output voltage and current at the load.

---

This is the final stage of the signal chain — PV panel → Buck MPPT → battery → Cascaded Boost → DC-AC Inverter → AC load — documented in the [power electronics lab overview](../).
