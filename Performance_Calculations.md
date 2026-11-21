
## Performance Calculations

**Technology:** GPDK 90 nm  
**Supply Voltage:** 1 V  
**Clock Frequency:** 100 MHz

The values in this section are analytical estimates. The supply voltage, clock frequency, device sizes, and circuit structure are based on the Cadence design. The resistance, capacitance, and leakage values are estimated and were not extracted from Cadence. Therefore, the results below are not Spectre-measured values.

### 1. Propagation Delay

Propagation delay is the time taken by the output to respond after an input transition. For an RC node, the 50% propagation delay is approximated by:

$$
t = 0.69 \times \tau
$$

where $\tau$ is the Elmore time constant.

**2-Input NAND Gate**

**High-to-Low (Series NMOS Pull-Down):**

$$
\tau_f = R_n C_x + 2R_n C_L
$$

$$
= (10\,k\Omega)(2\,fF) + (20\,k\Omega)(10\,fF)
$$

$$
= 0.22\,ns
$$

$$
t_{PHL} = 0.69 \times 0.22 \approx 0.15\,ns
$$

**Low-to-High (Worst Case, Only One PMOS Conducting):**

$$
\tau_r = R_p C_L
$$

$$
= (25\,k\Omega)(10\,fF) = 0.25\,ns
$$

$$
t_{PLH} = 0.69 \times 0.25 \approx 0.17\,ns
$$

**Average Propagation Delay:**

$$
t_p = \frac{t_{PHL}+t_{PLH}}{2}
$$

$$
= \frac{0.15+0.17}{2} = 0.16\,ns
$$

**3-Input NAND Gate**

**High-to-Low Transition:**

$$
\tau_f = R_n C_x + 2R_n C_x + 3R_n C_L
$$

$$
= 0.02 + 0.04 + 0.30 = 0.36\,ns
$$

$$
t_{PHL} = 0.69 \times 0.36 \approx 0.25\,ns
$$

**Low-to-High Transition:**

$$
\tau_r = 0.25\,ns
$$

$$
t_{PLH} = 0.69 \times 0.25 \approx 0.17\,ns
$$

**Average Propagation Delay:**

$$
t_p = \frac{0.25+0.17}{2} = 0.21\,ns
$$

**Master-Slave JK Flip-Flop (Clock-to-Q Delay)**

The clock-to-Q path is assumed to pass through three NAND2 stages. A rising output passes through two rising gate transitions and one falling transition, while a falling output passes through one rising and two falling transitions.

$$
t_{PLH(FF)} = 2(0.17)+0.15 = 0.49\,ns
$$

$$
t_{PHL(FF)} = 0.17+2(0.15) = 0.47\,ns
$$

$$
t_{p(FF)} = \frac{0.49+0.47}{2} = 0.48\,ns
$$

### 2. Elmore Delay

For a node driven through resistors, the Elmore time constant is calculated as the sum of each capacitance multiplied by the resistance shared with the output path:

$$
\tau = \sum_i C_i R_{i,\text{shared}}
$$

The values for one NAND2 gate are:

| Transition | Calculation | Elmore Time Constant ($\tau$) |
|---|---|---:|
| Pull-down (Series NMOS) | $10\,k\Omega \times 2\,fF + 20\,k\Omega \times 10\,fF$ | 0.22 ns |
| Pull-up (Single PMOS) | $25\,k\Omega \times 10\,fF$ | 0.25 ns |
| Average | $(0.22+0.25)/2$ | 0.235 ns |

Assuming three NAND2 stages per flip-flop and eight cascaded flip-flops, the time constants add along the ripple path:

$$
\tau_{FF} = 3 \times 0.235 = 0.705\,ns
$$

$$
\tau_{ripple} = 8 \times 0.705 = 5.64\,ns
$$

The corresponding 50% ripple delay is:

$$
t_{ripple} = 0.69 \times 5.64 \approx 3.89\,ns
$$

### 3. Ripple Delay

The ripple delay is estimated analytically from the NAND gate delays and is not measured in Spectre.

The approximate eight-stage ripple delay is:

$$
t_{ripple} = 8 \times t_{p(FF)}
$$

$$
= 8 \times 0.48 = \boxed{3.84\,ns}
$$

The Elmore estimate in Section 2 (3.89 ns) agrees with this result to within approximately 1.5%.

The worst-case transition occurs during the rollover from `01111111` to `10000000`, where Q0 to Q6 fall sequentially and Q7 rises last:

$$
t_{rollover} = 7 \times 0.47 + 0.49
$$

$$
= \boxed{3.78\,ns}
$$

The clock period is 10 ns, which is longer than the estimated ripple delay. Therefore, the outputs are expected to settle before the next clock cycle under these assumptions.

The estimated ripple delay corresponds to a theoretical frequency of:

$$
f_{max} \approx \frac{1}{3.84\,ns} \approx 260\,MHz
$$

This is an approximate timing limit, not a verified maximum operating frequency. Applications that decode the counter outputs must also account for temporary intermediate states during ripple propagation.

### 4. Dynamic Power

Dynamic power is the power consumed due to switching activity in the circuit.

$$
P_{dynamic} = C V_{DD}^{2} f
$$

This estimate considers only the eight Q output nodes. Each output Q(n) toggles at a frequency of:

$$
f_n = \frac{f}{2^{(n+1)}}
$$

Assuming a load capacitance of 10 fF for each of the eight Q outputs:

$$
P_{dynamic} = C V_{DD}^{2} f
\left(\frac{1}{2}+\frac{1}{4}+\cdots+\frac{1}{256}\right)
$$

$$
= (10\times10^{-15})(1)^2(100\times10^6)
(1-2^{-8})
$$

$$
= 0.9961\,\mu W
$$

$$
\boxed{P_{dynamic} \approx 1.00\,\mu W}
$$

Switching activity at the internal flip-flop nodes is not included in this estimate. Therefore, the actual total power may differ from the calculated output-node power.

### 5. Static Power

Static power is the power consumed due to leakage current when the circuit is not switching.

$$
P_{static} = V_{DD} \times I_{leakage}
$$

$$
= 1\,V \times 10\,nA
$$

$$
\boxed{P_{static} = 0.01\,\mu W}
$$

### 6. Average Power

The average power is the sum of dynamic and static power:

$$
P_{avg} = P_{dynamic} + P_{static}
$$

$$
= 0.9961 + 0.01
$$

$$
= 1.0061\,\mu W
$$

$$
\boxed{P_{avg} \approx 1.01\,\mu W}
$$

Dynamic power contributes approximately 99% of the estimated total power.

### 7. Power-Delay and Energy-Delay Product

**1. Power-Delay Product (PDP):**

PDP represents the product of average power and ripple propagation delay.

$$
PDP = P_{avg} \times t_{ripple}
$$

$$
= (1.01\times10^{-6})(3.84\times10^{-9})
$$

$$
\boxed{PDP = 3.88\times10^{-15}\,J = 3.88\,fJ}
$$

**2. Energy-Delay Product (EDP):**

EDP is the product of PDP and ripple propagation delay.

$$
EDP = PDP \times t_{ripple}
$$

$$
= (3.88\times10^{-15})(3.84\times10^{-9})
$$

$$
\boxed{EDP = 1.49\times10^{-23}\,J\cdot s}
$$

### 8. Performance Summary

| Parameter | Estimated Value |
|---|---:|
| Technology | GPDK 90 nm |
| Supply Voltage | 1 V |
| Clock Frequency | 100 MHz |
| $t_{PHL}$ (NAND2) | 0.15 ns |
| $t_{PLH}$ (NAND2) | 0.17 ns |
| Average Propagation Delay (NAND2) | 0.16 ns |
| Average Propagation Delay (NAND3) | 0.21 ns |
| Flip-Flop Clock-to-Q Delay | 0.48 ns |
| Elmore Time Constant (One NAND2) | 0.235 ns |
| Elmore Time Constant (Eight-Stage Ripple) | 5.64 ns |
| Eight-Stage Ripple Delay | 3.84 ns |
| Dynamic Power | 1.00 µW |
| Static Power | 0.01 µW |
| Average Power | 1.01 µW |
| PDP | 3.88 fJ |
| EDP | $1.49\times10^{-23}\,J\cdot s$ |
