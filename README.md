# 8-Bit Asynchronous Up Counter using Master-Slave JK Flip-Flops

**Author:** Mubeena Mirshad  
**Design and Simulation Environment:** Cadence Virtuoso  
**Project Area:** Digital IC Design / Sequential Logic

**Project Guide:** Dr. P Ranga Babu

![Cadence Virtuoso](https://img.shields.io/badge/EDA%20Tool-Cadence%20Virtuoso-blue.svg)
![Technology](https://img.shields.io/badge/Technology-GPDK%2090nm-green.svg)
![Design Flow](https://img.shields.io/badge/Flow-Full%20Custom%20IC%20Design-orange.svg)

A full-custom CMOS transistor-level and physical layout implementation of an **8-Bit Asynchronous (Ripple) Up Counter** designed using NAND-based **Master-Slave JK Flip-Flops** in **Cadence Virtuoso** (`gpdk090` 90nm process technology).

---

## Table of Contents

- [Overview](#overview)
- [Design Hierarchy & Architecture](#design-hierarchy--architecture)
- [Transistor Sizing & Technology Specifications](#transistor-sizing--technology-specifications)
- [Circuit Schematics & Physical Layouts](#circuit-schematics--physical-layouts)
  - [1. 2-Input CMOS NAND Gate](#1-2-input-cmos-nand-gate)
  - [2. 3-Input CMOS NAND Gate](#2-3-input-cmos-nand-gate)
  - [3. Master-Slave JK Flip-Flop](#3-master-slave-jk-flip-flop)
  - [4. Top-Level 8-Bit Asynchronous Up Counter](#4-top-level-8-bit-asynchronous-up-counter)
- [Transient Simulation Results](#transient-simulation-results)
- [Repository Structure](#repository-structure)
- [How to Reproduce](#how-to-reproduce)

---

## Overview

An **8-bit asynchronous up counter** counts binary states from `00000000` ($0_{10}$) to `11111111` ($255_{10}$), representing a **MOD-256** counter.

To eliminate the **race-around condition** associated with level-triggered JK flip-flops when $J = K = 1$, this design employs a Master-Slave JK Flip-Flop architecture built entirely from static CMOS NAND gates.

Each flip-flop is configured in toggle mode ($J = K = V_{DD}$). The output transition of each stage drives the clock input of the subsequent stage, enabling asynchronous binary up-counting.

---

## Design Hierarchy & Architecture

The design follows a bottom-up full-custom VLSI methodology:

1. **Standard Cell Primitives:**
   - **2-Input CMOS NAND Gate** (`pmos1v`, `nmos1v`)
   - **3-Input CMOS NAND Gate** (`pmos1v`, `nmos1v`)

2. **Sequential Building Block:**
   - **Master-Slave JK Flip-Flop:** Constructed using two 3-input NAND gates (input steering), six 2-input NAND gates (Master SR latch, Slave SR latch, and feedback steering), and one 2-input NAND gate wired as an inverter for the complementary slave clock.

3. **Top-Level System:**
   - **8-Bit Ripple Counter:** Eight cascaded Master-Slave JK Flip-Flop instances (`FF0` through `FF7`), producing outputs `Q0` (LSB) through `Q7` (MSB).

---

## Transistor Sizing & Technology Specifications

| Parameter | Specification |
|:---|:---|
| **EDA Suite** | Cadence Virtuoso (Schematic Editor, ADE L / Spectre, Layout Suite XL) |
| **Process Design Kit** | `gpdk090` (90nm Generic PDK) |
| **Device Models** | `gpdk090_pmos1v` / `gpdk090_nmos1v` |
| **Channel Length ($L$)** | $100\text{ nm}$ |
| **Channel Width ($W$)** | $120\text{ nm}$ (Multiplier $m = 1$) |
| **Supply Voltage ($V_{DD}$)** | $1.1\text{ V}$ |
| **Counting Range** | $0$ to $255$ (`0x00` to `0xFF`, 256 states) |

---

## Circuit Schematics & Physical Layouts

### 1. 2-Input CMOS NAND Gate

Static CMOS implementation using parallel PMOS pull-up transistors (`PM0`, `PM1`) and series NMOS pull-down transistors (`NM0`, `NM1`).

| Schematic View | Physical Layout View |
|:---:|:---:|
| ![2-Input NAND Schematic](Images/01_2input_nand_schematic.png) | ![2-Input NAND Layout](Images/02_2input_nand_layout.png) |

---

### 2. 3-Input CMOS NAND Gate

Static CMOS implementation using three parallel PMOS pull-up transistors (`PM0`–`PM2`) and three series NMOS pull-down transistors (`NM0`–`NM2`).

| Schematic View | Physical Layout View |
|:---:|:---:|
| ![3-Input NAND Schematic](Images/03_3input_nand_schematic.png) | ![3-Input NAND Layout](Images/04_3input_nand_layout.png) |

---

### 3. Master-Slave JK Flip-Flop

Gate-level schematic and full-custom physical layout integrating the Master and Slave stages with feedback paths (`Q` and `Qbar`) to achieve edge-triggered toggling without race-around instability.

**Gate-Level Schematic**

![Master-Slave JK Flip-Flop Schematic](Images/05_master_slave_jk_schematic.jpg)

**Physical Layout**

![Master-Slave JK Flip-Flop Layout](Images/06_master_slave_jk_layout.png)

---

### 4. 8-Bit Asynchronous Up Counter

Full-chip implementation of the 8-bit asynchronous up counter, consisting of eight cascaded Master-Slave JK Flip-Flop blocks arranged in a compact $2 \times 4$ array with inter-stage ripple clock routing.

**Schematic**

![8-Bit Asynchronous Counter Schematic](Images/01_8_bit_counter_schematic%20(1).jpg)

**Physical Layout**

![8-Bit Asynchronous Counter Layout](Images/07_8bit_asynchronous_counter_layout.png)

---

## Transient Simulation Results

Transient simulations were performed in **Cadence Spectre** with a $1.1\text{ V}$ square-wave input clock (`/clk`).

The waveforms demonstrate frequency division by $2^n$ at each successive output stage (`/Q0` through `/Q7`), verifying the 8-bit binary counting operation from `00000000` to `11111111`.

- **`/Q0` (LSB):** $f_{clk}/2$
- **`/Q1`:** $f_{clk}/4$
- **`/Q2`:** $f_{clk}/8$
- **`/Q3`:** $f_{clk}/16$
- **`/Q4`:** $f_{clk}/32$
- **`/Q5`:** $f_{clk}/64$
- **`/Q6`:** $f_{clk}/128$
- **`/Q7` (MSB):** $f_{clk}/256$

### Window 1 ($1.0\text{ }\mu s - 2.05\text{ }\mu s$): Lower-to-Mid Bit Transitions

![Transient Simulation Output 1](Images/08_simulation_waveform_1.jpg)

### Window 2 ($2.1\text{ }\mu s - 3.15\text{ }\mu s$): MSB (`/Q7`) Low-to-High Transition

![Transient Simulation Output 2](Images/09_simulation_waveform_2.jpg)

### Window 3 ($3.1\text{ }\mu s - 4.15\text{ }\mu s$): Full Rollover (`11111111` → `00000000`)

![Transient Simulation Output 3](Images/10_simulation_waveform_3.jpg)

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

1. Launch **Cadence Virtuoso** in a working directory configured with the `gpdk090` library.
2. Create a new library and build the leaf cells (`NAND2` and `NAND3`) using `pmos1v` and `nmos1v` ($W = 120\text{ nm}$, $L = 100\text{ nm}$), then generate their corresponding symbols and DRC/LVS-clean layouts.
3. Instantiate the NAND gates to construct the **Master-Slave JK Flip-Flop** schematic and layout, verifying functionality with $J = K = 1.1\text{ V}$.
4. Cascade eight instances of the Master-Slave JK Flip-Flop symbol to form the **8-Bit Asynchronous Up Counter**.
5. Open **ADE L**, select the **Spectre** simulator, set the transient stop time to $4.2\text{ }\mu s$, and plot `/clk` along with `/Q0`–`/Q7`.

---

## Author

**Mubeena Mirshad**

Digital IC Design | Full-Custom VLSI Design
