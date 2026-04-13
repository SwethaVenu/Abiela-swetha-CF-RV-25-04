# Performance Evaluation of Stage0 Variants in C-Class Core RTL

> **CEG Fabless RISC-V Summer Internship 2025**  
> College of Engineering, Guindy — Anna University, Chennai 600025  
> Submitted by: **Abiela Maria Y** (2022105503) & **Swetha V** (2022105013)  
> Supervised by: **Dr. Nitya Ranganathan** & **Mr. Sriram**, SHAKTI Team, IIT Madras

---

## 📋 Overview

This project evaluates the performance of **Stage 0** in the [SHAKTI C-Class RISC-V processor core](https://gitlab.com/shaktiproject/cores/c-class). Stage 0 consists of two critical pipeline components:

- **Program Counter Generator (PCGen)** — responsible for determining the next instruction address
- **Branch Predictor (GShare)** — responsible for predicting control flow to keep the pipeline filled

The study benchmarks the core in its default configuration, then modifies Stage 0 parameters (specifically Branch History Table depth) to analyze the effect on pipeline performance metrics such as CPI, instruction retirement count, and branch mispredictions.

---

## 🏗️ Architecture Summary

The SHAKTI C-Class is a **64-bit, 6-stage in-order RISC-V processor** supporting the `RV64GCSUN` ISA. It is highly configurable via YAML parameter files and is suitable for embedded systems through to Linux-based platforms.

### Stage-0 Configuration (Baseline)

| Parameter | Value |
|---|---|
| Predictor Type | GShare |
| BTB Depth | 32 entries |
| BHT Depth | 512 entries |
| History Length | 8 bits |
| History Bits | 5 |
| RAS Depth | 8 entries |

**Instruction Stream Buffer (ISB) sizes:**

| Stage Transition | Buffer Size |
|---|---|
| Stage 0 → Stage 1 | 2 entries |
| Stage 1 → Stage 2 | 2 entries |
| Stage 2 → Stage 3 | 1 entry |
| Stage 3 → Stage 4 | 8 entries |
| Stage 4 → Stage 5 | 8 entries |

---

## 🛠️ Setup & Build Instructions

### Prerequisites

- Ubuntu 20.04+ (or compatible Linux)
- Python 3, pip
- GHC (Glasgow Haskell Compiler)
- Git
- Verilator

### 1. Clone the C-Class Repository

```bash
git clone https://gitlab.com/shaktiproject/cores/c-class.git
cd c-class
```

### 2. Install Python Dependencies

```bash
pip install -r requirements.txt
```

### 3. Manage SoC Dependencies

```bash
repomanager --yaml $PWD/test_soc/c64_c32/c64_deps.yaml --verbose debug --clean
repomanager --yaml $PWD/test_soc/c64_c32/c64_deps.yaml --verbose debug --cup
```

### 4. Generate SoC Configuration

```bash
soc_config \
  -ispec sample_config/c64/rv64i_isa.yaml \
  -customspec sample_config/c64/rv64i_custom.yaml \
  -cspec sample_config/c64/core64.yaml \
  -gspec sample_config/c64/csr_grouping64.yaml \
  -dspec sample_config/c64/rv64i_debug.yaml \
  --verbose debug
```

### 5. Build the Core

```bash
export XXD_VERSION=2023
make generate_verilog
make link_verilator
make generate_boot_files
```

### 6. Export the Build Environment

```bash
export DESIGN_HOME=/home/vsysuser/workspace/uptickpro_examples/shakti_cclass/cclass/bin
```

---

## 🔧 Installing Supporting Tools

### Bluespec Compiler (BSC)

```bash
# Install GHC and required Haskell libraries
sudo apt-get install ghc
sudo apt-get install libghc-regex-compat-dev libghc-syb-dev libghc-old-time-dev libghc-split-dev

# Install build dependencies
sudo apt-get install tcl-dev build-essential pkg-config autoconf gperf flex bison iverilog

# Clone and build BSC
git clone --recursive https://github.com/B-Lang-org/bsc
cd bsc
make install-src
```

### RISC-V GNU Toolchain

```bash
git clone https://github.com/riscv/riscv-gnu-toolchain

# Install dependencies
sudo apt-get install autoconf automake autotools-dev curl python3 python3-pip \
  libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo \
  gperf libtool patchutils bc zlib1g-dev libexpat-dev ninja-build git cmake \
  libglib2.0-dev libslirp-dev

# Configure and build (Newlib cross-compiler)
./configure --prefix=$HOME/riscv --with-arch=rv64imafdc --with-abi=lp64d
make -j4 newlib

# Update PATH
export PATH=$HOME/riscv/bin:$PATH
```

---

## 🧪 Running Tests

```bash
cd ./../riscv_assembly
export DESIGN_HOME=/path/to/cclass/bin

# Run a test (replace with your test path)
make run_dut TEST=tests/branch.S
```

Benchmarks used in this project:
- **CoreMark** — general-purpose embedded benchmark
- **RISC-V standard test suites**
- **Custom microbenchmarks** provided by the SHAKTI team

---

## 📊 Performance Results

### Pre-Tuning (Baseline — BHT depth: 512)

| Metric | Hex Value | Decimal Value |
|---|---|---|
| Retired Instructions | `0x00000000c6991e` | 13,011,486 |
| Cycles | `0x00000000f712cc` | 16,167,244 |
| Branch Mispredictions | `0x0000000083658f` | 8,622,991 |
| Number of Jumps | `0x00000000024f07` | 151,559 |
| Number of Branches | `0x0000000020c392` | 2,147,474 |
| MUL/DIV Instructions | `0x0000000005bcd8` | 375,896 |

**CPI (baseline): ~1.24**

---

### Post-Tuning (BHT depth reduced: 512 → 500)

The BHT depth was reduced from 512 to 500 entries in `core64.yaml`:

```yaml
branch_predictor:
  instantiate: True
  predictor: gshare
  btb_depth: 32
  bht_depth: 500    # Modified from default 512
  history_len: 8
  history_bits: 5
  ras_depth: 8
```

| Metric | Hex Value | Decimal Value |
|---|---|---|
| Retired Instructions | `0x0000000000011000` | 69,632 |
| Cycles | `0x0000000000011300` | 70,400 |
| Branch Mispredictions | `0x000000000001130c` | 70,412 |
| Number of Jumps | `0x0000000000000001` | 1 |
| MUL/DIV Instructions | `0x000000000001130c` | 70,412 |

---

## 💡 Key Findings

- Even a **small reduction in BHT depth** (512 → 500 entries) leads to a measurable increase in branch mispredictions due to higher aliasing in the GShare predictor table.
- Branch predictor performance in GShare is highly sensitive to table size — different branches with similar history patterns compete for the same index when the table is undersized.
- Increased mispredictions cause **more pipeline flushes**, which accumulate into significant performance penalties in branch-intensive workloads.
- The baseline CPI of ~1.24 is acceptable but can be degraded by poor branch predictor configuration.
- These results highlight the importance of **careful microarchitectural parameter tuning** during design space exploration for open-source RISC-V cores.

---

## 📁 Repository Structure

```
.
├── sample_config/
│   └── c64/
│       ├── core64.yaml          # Core configuration (modify Stage-0 params here)
│       ├── rv64i_isa.yaml
│       ├── rv64i_custom.yaml
│       ├── csr_grouping64.yaml
│       └── rv64i_debug.yaml
├── test_soc/
│   └── c64_c32/
│       └── c64_deps.yaml
├── riscv_assembly/
│   └── tests/                   # Test programs
├── build/
│   └── hw/verilog/              # Generated Verilog RTL
├── bin/                         # Simulation executables
└── README.md
```

---

## 📚 References

- [SHAKTI C-Class Core](https://gitlab.com/shaktiproject/cores/c-class)
- [Bluespec Compiler (BSC)](https://github.com/B-Lang-org/bsc)
- [RISC-V GNU Toolchain](https://github.com/riscv/riscv-gnu-toolchain)
- [SHAKTI Benchmarks](https://gitlab.com/shaktiproject/cores/benchmarks)
- [UptickPro Example Designs](https://github.com/vyomasystems/uptickpro_examples)

---

## 🙏 Acknowledgements

We express our sincere gratitude to **Dr. K.S. Easwarakumar** (Dean, CEG), **Dr. M.A. Bhagyaveni** (HoD, ECE), our supervisors **Dr. Nitya Ranganathan** and **Mr. Sriram** from the SHAKTI Team at IIT Madras, and **Mr. Sivakumar Anandan** (CEG alumnus) for bringing us this opportunity.
