## Performance Calculations

Technology: GPDK 90 nm, supply voltage 1 V, clock frequency 100 MHz.

The values in this section are analytical estimates. The supply voltage, clock frequency, device sizes and circuit structure come from the Cadence design. The resistance, capacitance and leakage values are estimated and were not extracted from Cadence, so the results below are not Spectre-measured values.


### 1. Propagation Delay

The propagation delay is the time the output takes to respond after the input changes. For an RC node the 50% delay is t = 0.69 · τ, where τ is the Elmore time constant (see section 2).

**2-input NAND**

High-to-low (series NMOS pull-down):

τ<sub>f</sub> = R<sub>n</sub>·C<sub>x</sub> + 2R<sub>n</sub>·C<sub>L</sub> = (10 kΩ)(2 fF) + (20 kΩ)(10 fF) = 0.22 ns

t<sub>PHL</sub> = 0.69 × 0.22 ns ≈ 0.15 ns

Low-to-high (worst case, only one PMOS conducting):

τ<sub>r</sub> = R<sub>p</sub>·C<sub>L</sub> = (25 kΩ)(10 fF) = 0.25 ns

t<sub>PLH</sub> = 0.69 × 0.25 ns ≈ 0.17 ns

Average propagation delay:

t<sub>p</sub> = (t<sub>PHL</sub> + t<sub>PLH</sub>) / 2 = (0.15 + 0.17) / 2 = 0.16 ns

**3-input NAND**

τ<sub>f</sub> = R<sub>n</sub>·C<sub>x</sub> + 2R<sub>n</sub>·C<sub>x</sub> + 3R<sub>n</sub>·C<sub>L</sub> = 0.02 + 0.04 + 0.30 = 0.36 ns, so t<sub>PHL</sub> ≈ 0.25 ns

τ<sub>r</sub> = 0.25 ns, so t<sub>PLH</sub> ≈ 0.17 ns

t<sub>p</sub> = (0.25 + 0.17) / 2 = 0.21 ns

**Master-slave JK flip-flop (clock to Q)**

The path from the clock to Q goes through three NAND2 stages. A rising output passes through two rising gate transitions and one falling, and a falling output through one rising and two falling:

t<sub>PLH(FF)</sub> = 2 × 0.17 + 0.15 = 0.49 ns

t<sub>PHL(FF)</sub> = 0.17 + 2 × 0.15 = 0.47 ns

t<sub>p(FF)</sub> = (0.49 + 0.47) / 2 = 0.48 ns

### 2. Elmore Delay

For a node driven through resistors in series, the Elmore delay is the sum of each node capacitance multiplied by the resistance it shares with the output path:

τ = Σ C<sub>i</sub> · R<sub>i,shared</sub>

The values for one NAND2 gate are:

| Transition | Calculation | τ |
|:---|:---|:---:|
| Pull-down (series NMOS) | 10 kΩ × 2 fF + 20 kΩ × 10 fF | 0.22 ns |
| Pull-up (single PMOS) | 25 kΩ × 10 fF | 0.25 ns |
| Average | (0.22 + 0.25) / 2 | 0.235 ns |

Each flip-flop output is driven by a gate, so the stages are isolated from one another and their time constants simply add along the ripple path:

τ<sub>FF</sub> = 3 × 0.235 ns = 0.705 ns

τ<sub>ripple</sub> = 8 × τ<sub>FF</sub> = 8 × 0.705 ns = 5.64 ns

t<sub>ripple</sub> = 0.69 × 5.64 ns ≈ 3.89 ns

### 3. Ripple Delay

The ripple delay is estimated analytically from the NAND gate delays above. It is not measured in Spectre. The approximate eight-stage ripple delay is:

t<sub>ripple</sub> = 8 × t<sub>p(FF)</sub> = 8 × 0.48 ns = **3.84 ns**

The Elmore result in section 2 (3.89 ns) agrees with this to within about 1.5%.

The worst case is the rollover `01111111` to `10000000`, where Q0 to Q6 fall one after another and Q7 rises last: 7 × 0.47 + 0.49 = 3.78 ns, which is also close.

The clock period (10 ns) is longer than the ripple delay, so all outputs settle before the next count. The ripple delay limits the clock frequency to roughly 1 / 3.84 ns ≈ 260 MHz, and only if the outputs are decoded. While the ripple travels the outputs briefly show intermediate codes, so anything that decodes the counter output has to allow for this.

### 4. Dynamic Power

Dynamic power is the power used while the outputs switch:

P<sub>dynamic</sub> = C · V<sub>DD</sub>² · f

This estimate includes only the eight Q output nodes. Output Q(n) switches at $f/2^{(n+1)}$, so the activity halves at every stage. Taking C = 10 fF for each of the eight Q outputs:

P<sub>dynamic</sub> = C · V<sub>DD</sub>² · f · (1/2 + 1/4 + ... + 1/256)

= (10 × 10⁻¹⁵)(1)²(100 × 10⁶)(1 − 2⁻⁸)

= 1.00 µW (0.996 µW before rounding)

Switching of the internal flip-flop nodes is not included, so the power simulated in Spectre will be higher.

### 5. Static Power

Static power is the power used when the circuit is not switching:

P<sub>static</sub> = V<sub>DD</sub> × I<sub>leakage</sub> = 1 V × 10 nA = 0.01 µW

### 6. Average Power

P<sub>avg</sub> = P<sub>dynamic</sub> + P<sub>static</sub> = 1.00 + 0.01 = 1.01 µW

Dynamic power is about 99% of the total.

### 7. Power-Delay and Energy-Delay Product

PDP = P<sub>avg</sub> × t<sub>ripple</sub> = (1.01 × 10⁻⁶ W)(3.84 × 10⁻⁹ s) = 3.88 × 10⁻¹⁵ J = 3.88 fJ

EDP = PDP × t<sub>ripple</sub> = (3.88 × 10⁻¹⁵ J)(3.84 × 10⁻⁹ s) = 1.49 × 10⁻²³ J·s (14.9 fJ·ns)

### 8. Performance Summary

| Parameter | Estimated Value |
|:---|:---:|
| Supply voltage | 1 V |
| Clock frequency | 100 MHz |
| t<sub>PHL</sub> (NAND2) | 0.15 ns |
| t<sub>PLH</sub> (NAND2) | 0.17 ns |
| Average propagation delay (NAND2) | 0.16 ns |
| Flip-flop clock-to-Q delay | 0.48 ns |
| Elmore time constant, one NAND2 (average) | 0.235 ns |
| Elmore time constant, eight-stage ripple | 5.64 ns |
| Eight-stage ripple delay | 3.84 ns |
| Dynamic power | 1.00 µW |
| Static power | 0.01 µW |
| Average power | 1.01 µW |
| PDP | 3.88 fJ |
| EDP | 1.49 × 10⁻²³ J·s |

