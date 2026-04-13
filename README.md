# C-Class RISC-V Core — Stage 0 Performance Analysis

> **Branch Predictor and PCGen tuning study** on the Shakti C-Class core using CoreMark and RISC-V benchmarks

---

## Project Overview

This project analyses the **Stage 0 pipeline** of the Shakti C-Class RISC-V core by systematically varying configuration knobs in  and measuring the impact on execution metrics via BSV (Bluespec Verilog) simulation.

**Focus areas:**
- Program Counter Generator (PCGen)
- Branch Predictor (GShare + BTB + BHT + RAS)

---

## Hardware and Simulation Environment

| Component | Details |
|---|---|
| Core | Shakti C-Class (RV64GC) |
| RTL Language | Bluespec Verilog (BSV) |
| Simulator | BSV simulation (bsc) |
| Benchmarks | CoreMark, RISC-V microbenchmarks (Shakti team) |
| Configuration file |  |

---

## Stage 0 Parameters Explored

| Parameter | Default | Description |
|---|---|---|
|  |  | Enable/disable branch predictor |
|  |  | Predictor algorithm |
|  |  | Branch Target Buffer depth |
|  |  | Branch History Table depth |
|  |  | Global history length |
|  |  | Bits used to index into BHT |
|  |  | Return Address Stack depth |
|  | -- | Initial Program Counter value |

---

## Baseline Execution Results (Default )

Measured from  before any parameter modifications:

| Metric | Hex Value | Decimal |
|---|---|---|
| Retired Instructions |  | 13,011,486 |
| Cycles |  | 16,167,244 |
| Branch Mispredictions |  | 8,622,991 |
| Number of Jumps |  | 151,559 |
| Number of Branches |  | 2,147,474 |
| MUL/DIV Instructions |  | 375,896 |

**IPC (baseline):** 

---

## BHT Depth Experiment: 512 to 500

Modified  from **512** to **500** in :



### Results after modification ():

| Metric | Hex Value | Decimal |
|---|---|---|
| Retired Instructions |  | 69,632 |
| Cycles |  | 70,400 |
| Branch Mispredictions |  | 70,412 |
| Number of Jumps |  | 1 |
| MUL/DIV Instructions |  | 70,412 |

> This change demonstrates the sensitivity of prediction accuracy and execution metrics to BHT depth.

---

## Sample RTL Trace



---

## Project Files

| File | Description |
|---|---|
|  | Full project report |
|  | Presentation slides |
|  | Methodology and project workflow |
|  | Week 2 progress -- parameter identification and BSV setup |
|  | BHT depth experiment details and results |
|  | CoreMark baseline results and RTL trace |

---

## Week 2 Progress Summary

- Installed Bluespec Verilog toolchain
- Built C-Class core with default 
- Identified 8 Stage 0 parameters for systematic study
- Ran baseline simulation and captured CSR performance counters from 
- Modified  (512 to 500) and re-ran simulation
- Actively debugging simulation errors from initial run

---

## References

- [Shakti C-Class Core](https://gitlab.com/shaktiproject/cores/c-class)
- [CoreMark Benchmark - EEMBC](https://www.eembc.org/coremark/)
- [Bluespec Verilog (BSV)](https://github.com/B-Lang-org/bsc)
- [RISC-V ISA Specification](https://riscv.org/technical/specifications/)

---

## Team

| Name | Role |
|---|---|
| AbielaMaria | Contributor |
| SwethaVenu | Contributor |

**Course:** CF-RV-25-04 -- RISC-V Core Design and Performance Analysis
