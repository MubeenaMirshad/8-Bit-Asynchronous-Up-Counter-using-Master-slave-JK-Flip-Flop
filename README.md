
# 8-Bit Asynchronous Up Counter using Master-Slave JK Flip-Flops

**Author:** Mubeena Mirshad  
**Design and Simulation Environment:** Cadence Virtuoso  
**Project Area:** Digital IC Design / Sequential Logic  
**Project Guide:** Dr. P Ranga Babu

![Cadence Virtuoso](https://img.shields.io/badge/EDA%20Tool-Cadence%20Virtuoso-blue.svg)
![Technology](https://img.shields.io/badge/Technology-GPDK%2090nm-green.svg)
![Design Flow](https://img.shields.io/badge/Flow-Full%20Custom%20IC%20Design-orange.svg)

This project presents the design and implementation of an **8-bit asynchronous (ripple) up counter** using NAND-based Master-Slave JK flip-flops. The design includes transistor-level schematic development, CMOS physical layout implementation, and transient simulation using Cadence Virtuoso with the GPDK 90nm technology.

---

## Table of Contents

- [Overview](#overview)
- [Key Specifications](#key-specifications)
- [Design Hierarchy and Architecture](#design-hierarchy-and-architecture)
- [Transistor Sizing and Technology Specifications](#transistor-sizing-and-technology-specifications)
- [Circuit Schematics and Physical Layouts](#circuit-schematics-and-physical-layouts)
  - [1. 2-Input CMOS NAND Gate](#1-2-input-cmos-nand-gate)
  - [2. 3-Input CMOS NAND Gate](#2-3-input-cmos-nand-gate)
  - [3. Master-Slave JK Flip-Flop](#3-master-slave-jk-flip-flop)
  - [4. 8-Bit Asynchronous Up Counter](#4-8-bit-asynchronous-up-counter)
- [Transient Simulation Results](#transient-simulation-results)
- [Timing Performance Analysis](#timing-performance-analysis)
- [Repository Structure](#repository-structure)
- [How to Reproduce](#how-to-reproduce)
- [Conclusion](#conclusion)
- [Author](#author)

---

## Overview

An **8-bit asynchronous up counter** counts binary states from `00000000` (0 in decimal) to `11111111` (255 in decimal), providing a total of 256 states. Therefore, it operates as a **MOD-256 counter**.

The design uses Master-Slave JK flip-flops to avoid the race-around condition associated with level-triggered JK flip-flops when both J and K inputs are HIGH.

Each flip-flop is configured in toggle mode by connecting:

\[
J=K=V_{DD}
\]

When triggered, the flip-flop changes its output state. The output transition of one stage drives the clock input of the next stage, producing asynchronous ripple counting.

The counter consists of eight cascaded flip-flops, with `Q0` representing the least significant bit (LSB) and `Q7` representing the most significant bit (MSB).

---

## Key Specifications

| Parameter | Specification |
|:---|:---|
| Technology Node | 90 nm CMOS (GPDK90) |
| Supply Voltage (\(V_{DD}\)) | 1.1 V |
| Counter Architecture | 8-bit Asynchronous Up Counter |
| Flip-Flop Type | Master-Slave JK |
| Logic Implementation | Static CMOS NAND Gates |
| Number of Flip-Flops | 8 |
| Counting Sequence | 00000000 to 11111111 |
| Counting Modulus | MOD-256 |
| Clock Frequency | 50 MHz (assumed) |
| Clock Period | 20 ns |

---

## Design Hierarchy and Architecture

The design follows a bottom-up full-custom VLSI design approach.

### 1. Basic Logic Gates

- **2-Input CMOS NAND Gate:** Implemented using two PMOS and two NMOS transistors.
- **3-Input CMOS NAND Gate:** Implemented using three PMOS and three NMOS transistors.

### 2. Sequential Building Block

**Master-Slave JK Flip-Flop:**

The flip-flop is constructed using NAND gates arranged into master and slave latch sections with input steering and feedback connections.

The implementation consists of:

- Two 3-input NAND gates for input steering.
- Six 2-input NAND gates for the master and slave latch and feedback logic.
- One additional 2-input NAND gate configured as an inverter for the complementary clock signal.

### 3. Top-Level Counter

**8-Bit Asynchronous Up Counter:**

Eight Master-Slave JK flip-flop instances (`FF0` through `FF7`) are connected in cascade.

All flip-flops operate in toggle mode with J and K connected HIGH. The clock connection between successive stages enables ripple-based binary counting.

The outputs are:

- `Q0` – Least Significant Bit (LSB)
- `Q1` to `Q6` – Intermediate bits
- `Q7` – Most Significant Bit (MSB)

---

## Transistor Sizing and Technology Specifications

| Parameter | Specification |
|:---|:---|
| EDA Suite | Cadence Virtuoso |
| Schematic Editor | Virtuoso Schematic Editor |
| Simulation Environment | ADE L / Spectre |
| Layout Environment | Virtuoso Layout Suite XL |
| Process Design Kit | `gpdk090` |
| Technology | 90 nm CMOS |
| NMOS Model | `gpdk090_nmos1v` |
| PMOS Model | `gpdk090_pmos1v` |
| Channel Length (\(L\)) | 100 nm |
| Channel Width (\(W\)) | 120 nm |
| Multiplier (\(m\)) | 1 |
| Supply Voltage (\(V_{DD}\)) | 1.1 V |

The transistor dimensions are selected for the CMOS NAND gate implementation and used throughout the design as specified in the respective transistor instances.

---

## Circuit Schematics and Physical Layouts

### 1. 2-Input CMOS NAND Gate

The 2-input NAND gate is implemented using static CMOS logic.

It consists of two PMOS transistors connected in parallel to form the pull-up network and two NMOS transistors connected in series to form the pull-down network.

**Schematic and Physical Layout**

| Schematic View | Physical Layout View |
|:---:|:---:|
| ![2-Input NAND Schematic](Images/01_2input_nand_schematic.png) | ![2-Input NAND Layout](Images/02_2input_nand_layout.png) |

---

### 2. 3-Input CMOS NAND Gate

The 3-input NAND gate is implemented using three parallel PMOS transistors and three series NMOS transistors.

The output becomes LOW only when all three inputs are HIGH.

**Schematic and Physical Layout**

| Schematic View | Physical Layout View |
|:---:|:---:|
| ![3-Input NAND Schematic](Images/03_3input_nand_schematic.png) | ![3-Input NAND Layout](Images/04_3input_nand_layout.png) |

---

### 3. Master-Slave JK Flip-Flop

The Master-Slave JK flip-flop is constructed using NAND gates with master and slave latch sections.

The feedback connections provide the required JK operation, while the complementary clock signal controls the transfer of data between the two stages.

When both J and K inputs are HIGH, the output toggles at each active triggering edge.

**Gate-Level Schematic**

![Master-Slave JK Flip-Flop Schematic](Images/05_master_slave_jk_schematic.jpg)

**Physical Layout**

![Master-Slave JK Flip-Flop Layout](Images/06_master_slave_jk_layout.png)

---

### 4. 8-Bit Asynchronous Up Counter

The complete counter is constructed by cascading eight Master-Slave JK flip-flops.

Each stage operates in toggle mode, and the output of one stage provides the clock signal for the following stage. This produces the binary counting sequence.

The eight flip-flop blocks are arranged in a 2 × 4 configuration.

**Top-Level Schematic**

![8-Bit Asynchronous Counter Schematic](Images/01_8_bit_counter_schematic%20(1).jpg)

**Physical Layout**

![8-Bit Asynchronous Counter Layout](Images/07_8bit_asynchronous_counter_layout.png)

---

## Transient Simulation Results

Transient simulation was performed using **Cadence Spectre** to verify the operation of the 8-bit asynchronous up counter.

A 1.1 V square-wave clock signal (`/clk`) was applied to the first flip-flop. The output waveforms of the successive stages were observed to verify the counting operation.

Each successive output divides the frequency of the preceding stage by two.

| Output | Frequency Relationship |
|:---|:---|
| `/Q0` (LSB) | \(f_{clk}/2\) |
| `/Q1` | \(f_{clk}/4\) |
| `/Q2` | \(f_{clk}/8\) |
| `/Q3` | \(f_{clk}/16\) |
| `/Q4` | \(f_{clk}/32\) |
| `/Q5` | \(f_{clk}/64\) |
| `/Q6` | \(f_{clk}/128\) |
| `/Q7` (MSB) | \(f_{clk}/256\) |

For an assumed clock frequency of 50 MHz, the corresponding output frequencies are:

| Output | Frequency |
|:---|---:|
| Q0 | 25 MHz |
| Q1 | 12.5 MHz |
| Q2 | 6.25 MHz |
| Q3 | 3.125 MHz |
| Q4 | 1.5625 MHz |
| Q5 | 781.25 kHz |
| Q6 | 390.625 kHz |
| Q7 | 195.3125 kHz |

### Simulation Waveform 1 – Lower and Intermediate Bit Transitions

This waveform shows the switching activity of the counter outputs and the frequency division across the lower and intermediate stages.

![Transient Simulation Output 1](Images/08_simulation_waveform_1.jpg)

### Simulation Waveform 2 – MSB Transition

This waveform shows the transition of the higher-order counter outputs, including the MSB (`/Q7`).

![Transient Simulation Output 2](Images/09_simulation_waveform_2.jpg)

### Simulation Waveform 3 – Higher-Order Output Transitions

This waveform provides another view of the output transitions during the counting operation.

![Transient Simulation Output 3](Images/10_simulation_waveform_3.jpg)

**Expected Counter Operation**

The counter follows the binary sequence:

```text
00000000
00000001
00000010
00000011
00000100
    .
    .
    .
11111110
11111111
00000000
```

For a 50 MHz clock, the time required for 256 clock cycles is:

\[
T_{rollover}=256\times20ns
\]

\[
\boxed{T_{rollover}=5.12\ \mu s}
\]

Therefore, a transient simulation starting from zero must extend beyond 5.12 µs to capture the complete MOD-256 rollover.

---

## Timing Performance Analysis

The propagation delay of each stage contributes to the overall ripple delay of the asynchronous counter.

The following values are illustrative theoretical estimates and are not extracted Cadence measurements.

### 1. Estimated Propagation Delay

| Parameter | Symbol | Estimated Value |
|:---|:---|---:|
| High-to-Low Delay | \(t_{PHL}\) | 0.18 ns |
| Low-to-High Delay | \(t_{PLH}\) | 0.22 ns |
| Average Propagation Delay | \(t_{pd}\) | 0.20 ns |

The average propagation delay is calculated as:

\[
t_{pd}=\frac{t_{PLH}+t_{PHL}}{2}
\]

\[
t_{pd}=\frac{0.22+0.18}{2}
\]

\[
\boxed{t_{pd}=0.20\ ns}
\]

### 2. Estimated Ripple Propagation Delay

For an approximate eight-stage ripple propagation path:

\[
t_{ripple}=N\times t_{pd}
\]

Where:

- \(N=8\) is the number of flip-flop stages.
- \(t_{pd}=0.20\ ns\) is the assumed average stage delay.

\[
t_{ripple}=8\times0.20
\]

\[
\boxed{t_{ripple}=1.60\ ns}
\]

This is a first-order estimate. The actual ripple delay depends on the clock-to-output delay of each flip-flop, loading, and interconnect parasitics.

### 3. Timing Reference

The timing measurements, if extracted from simulation, should use the 50% supply-voltage threshold.

For the specified 1.1 V supply:

\[
V_{mid}=0.5\times1.1
\]

\[
\boxed{V_{mid}=0.55\ V}
\]

The estimated delay values above should not be interpreted as measured timing results.

**Note:** Actual propagation delay, contamination delay, setup time, hold time, and power dissipation require separate characterization using the appropriate Cadence simulation analyses.

---

## Repository Structure

```text
8-bit-asynchronous-up-counter/
│
├── README.md
│
├── Images/
│   ├── 01_2input_nand_schematic.png
│   ├── 01_8_bit_counter_schematic (1).jpg
│   ├── 02_2input_nand_layout.png
│   ├── 03_3input_nand_schematic.png
│   ├── 04_3input_nand_layout.png
│   ├── 05_master_slave_jk_schematic.jpg
│   ├── 06_master_slave_jk_layout.png
│   ├── 07_8bit_asynchronous_counter_layout.png
│   ├── 08_simulation_waveform_1.jpg
│   ├── 09_simulation_waveform_2.jpg
│   └── 10_simulation_waveform_3.jpg
│
└── docs/
```

---

## How to Reproduce

1. Launch **Cadence Virtuoso** in a working directory configured with the `gpdk090` technology library.

2. Create a new design library and develop the basic CMOS NAND gates (`NAND2` and `NAND3`) using the `pmos1v` and `nmos1v` devices.

3. Use the specified transistor dimensions (\(W=120\ nm\), \(L=100\ nm\)) and create the corresponding schematic symbols and physical layouts.

4. Construct the Master-Slave JK flip-flop using the NAND gate building blocks. Configure the J and K inputs in toggle mode by connecting them to the supply voltage.

5. Verify the JK flip-flop operation through transient simulation.

6. Instantiate eight JK flip-flops and connect them in cascade to construct the 8-bit asynchronous up counter. Ensure that the inter-stage clock connections match the triggering polarity required for up-counting.

7. Open **ADE L**, select the **Spectre** simulator, configure the transient analysis, and apply the clock input.

8. Set the transient stop time to approximately 5.5–6 µs for a 50 MHz clock to observe the complete MOD-256 counting sequence, including rollover.

9. Plot the input clock (`/clk`) and output signals (`/Q0` to `/Q7`) to verify the counter operation.

---

## Conclusion

This project demonstrates the design and implementation of an 8-bit asynchronous up counter using NAND-based Master-Slave JK flip-flops in Cadence Virtuoso. The design includes CMOS NAND gate development, flip-flop construction, transistor-level schematic implementation, and physical layout design using the GPDK 90nm technology.

Transient simulation is used to verify the binary counting operation and frequency division across successive output stages. Through this project, practical experience was gained in CMOS circuit design, sequential logic, physical layout implementation, and circuit-level simulation.

---

## Author

**Mubeena Mirshad**

Digital IC Design | Full-Custom VLSI Design
