# Preliminary Performance Calculations

## 8-Bit Asynchronous Up Counter Using Master-Slave JK Flip-Flops

**Design environment:** Cadence Virtuoso / Spectre  
**Technology stated in repository README:** GPDK 90 nm  
**Device dimensions stated in repository README:** W = 120 nm, L = 100 nm  
**Supply voltage used below:** VDD = 1.0 V

> **Important:** These are illustrative preliminary estimates, not values measured from Cadence. The supplied screenshots do not show the 50% crossing measurements, supply-current waveform, or extracted capacitances. All assumptions are listed below.

## 1. Assumptions

| Quantity | Assumed value | Note |
|---|---:|---|
| Supply voltage, VDD | 1.0 V | Based on the user's stated schematic supply |
| Clock frequency, f | 50 MHz | Assumed; verify from the VPULSE period |
| Effective switched capacitance, Ceff | 10 fF | Illustrative, not extracted |
| Switching coefficient, k | 2 | Simplified activity multiplier |
| Leakage current, Ileak | 10 nA | Illustrative, not measured |
| Estimated per-stage tPHL | 0.18 ns | Illustrative timing estimate |
| Estimated per-stage tPLH | 0.22 ns | Illustrative timing estimate |

## 2. Propagation delay

Propagation delay is measured between the 50% voltage points of the triggering clock edge and the corresponding output edge.

### High-to-low delay

Assumed:

\[
t_{PHL}=0.18\ \text{ns}
\]

### Low-to-high delay

Assumed:

\[
t_{PLH}=0.22\ \text{ns}
\]

### Average delay per flip-flop stage

\[
t_p=\frac{t_{PHL}+t_{PLH}}{2}
\]

\[
t_p=\frac{0.18+0.22}{2}=\boxed{0.20\ \text{ns}}
\]

### Approximate eight-stage ripple delay

For a simplified estimate in which all eight stages contribute one average stage delay:

\[
t_{ripple}\approx 8t_p=8(0.20)=\boxed{1.60\ \text{ns}}
\]

This is a first-order estimate. Actual ripple settling time depends on the triggering edge, stage clock-to-output delays, loading, and interconnect parasitics.

## 3. Dynamic power

Using the simplified switching-power expression:

\[
P_{dynamic}=kC_{eff}V_{DD}^{2}f
\]

Substitute the assumed values:

\[
P_{dynamic}=2(10\times10^{-15})(1)^2(50\times10^6)
\]

\[
P_{dynamic}=1.0\times10^{-6}\ \text{W}
\]

\[
\boxed{P_{dynamic}=1.00\ \mu\text{W}}
\]

## 4. Static power

Static power is estimated from the assumed leakage current:

\[
P_{static}=V_{DD}I_{leak}
\]

\[
P_{static}=1(10\times10^{-9})
=10\times10^{-9}\ \text{W}
\]

\[
\boxed{P_{static}=0.01\ \mu\text{W}}
\]

## 5. Average power

For this simplified estimate:

\[
P_{avg}\approx P_{dynamic}+P_{static}
\]

\[
P_{avg}\approx1.00+0.01
\]

\[
\boxed{P_{avg}\approx1.01\ \mu\text{W}}
\]

The direct simulation-based method is to average the supply power over a time interval:

\[
P_{avg}=\frac{1}{T}\int_0^T V_{DD}I_{DD}(t)\,dt
\]

## 6. Summary of illustrative estimates

| Parameter | Illustrative estimate |
|---|---:|
| Per-stage tPHL | 0.18 ns |
| Per-stage tPLH | 0.22 ns |
| Average per-stage propagation delay | 0.20 ns |
| Approximate eight-stage ripple delay | 1.60 ns |
| Dynamic power | 1.00 µW |
| Static power | 0.01 µW |
| Average power | 1.01 µW |

## 7. Notes before reporting

1. The repository README currently lists **VDD = 1.1 V**, while the schematic screenshot and user's latest message indicate **1.0 V**. This calculation sheet uses **1.0 V**. Confirm the actual ADE supply setting.
2. The 50 MHz clock, 10 fF effective capacitance, switching coefficient of 2, and 10 nA leakage current are assumptions; they were not recovered from the Cadence setup.
3. The delay values are illustrative, not measured from the displayed waveforms. Actual tPHL and tPLH require clock-to-output measurements at the 50% VDD crossing points.
4. Actual dynamic, static, and average power require the supply-current waveform and real clock/load conditions. Do not describe these assumed values as Cadence simulation results.
5. Replace the assumed quantities with ADE/Spectre values when available and repeat the calculations.
