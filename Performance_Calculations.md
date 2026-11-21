
# Performance Calculations

## 8-Bit Asynchronous Up Counter Using Master-Slave JK Flip-Flops

**Technology:** GPDK 90nm  
**Supply Voltage:** 1 V  
**EDA Tool:** Cadence Virtuoso

---

## 1. Propagation Delay

The propagation delay is the time taken by the output to respond to a change in the input.

### High-to-Low Propagation Delay

$$
t_{PHL}=0.18\ ns
$$

### Low-to-High Propagation Delay

$$
t_{PLH}=0.22\ ns
$$

### Average Propagation Delay

$$
t_p=\frac{t_{PHL}+t_{PLH}}{2}
$$

$$
t_p=\frac{0.18+0.22}{2}
$$

$$
\boxed{t_p=0.20\ ns}
$$

### Approximate Eight-Stage Ripple Delay

$$
t_{ripple}=8\times t_p
$$

$$
t_{ripple}=8\times0.20
$$

$$
\boxed{t_{ripple}=1.60\ ns}
$$

---

## 2. Dynamic Power

Dynamic power is the power consumed during switching operations.

$$
P_{dynamic}=\alpha C_{eff}V_{DD}^{2}f
$$

Assuming:

- Switching coefficient = 2
- Effective capacitance = 10 fF
- Supply voltage = 1 V
- Clock frequency = 50 MHz

Substituting:

$$
P_{dynamic}=2\times10\times10^{-15}
\times1^2\times50\times10^6
$$

$$
\boxed{P_{dynamic}=1.00\ \mu W}
$$

---

## 3. Static Power

Static power is the power consumed when the circuit is not switching.

$$
P_{static}=V_{DD}\times I_{leakage}
$$

Assuming leakage current = 10 nA,

$$
P_{static}=1\times10\times10^{-9}
$$

$$
\boxed{P_{static}=0.01\ \mu W}
$$

---

## 4. Average Power

Average power is calculated as the sum of dynamic and static power.

$$
P_{avg}=P_{dynamic}+P_{static}
$$

$$
P_{avg}=1.00+0.01
$$

$$
\boxed{P_{avg}=1.01\ \mu W}
$$

---

## 5. Performance Summary

| Parameter | Estimated Value |
|:---|---:|
| Supply Voltage | 1 V |
| tPHL | 0.18 ns |
| tPLH | 0.22 ns |
| Average Propagation Delay | 0.20 ns |
| Eight-Stage Ripple Delay | 1.60 ns |
| Dynamic Power | 1.00 µW |
| Static Power | 0.01 µW |
| Average Power | 1.01 µW |

---

## Note

The above values are preliminary theoretical estimates based on assumed operating conditions. They are not directly measured Cadence simulation results. Actual performance depends on clock frequency, load capacitance, transistor characteristics, and leakage current.
