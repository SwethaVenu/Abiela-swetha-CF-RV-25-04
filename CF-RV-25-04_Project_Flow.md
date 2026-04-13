**Problem Statement**  
**C-Class Core Performance Analysis: Stage 0 (PCGen, Branch Predictor)**  

The objective of this project is to analyze the performance of the C-Class RISC-V core, specifically focusing on Stage 0, which includes the Program Counter Generator (PCGen) and the Branch Predictor.  

We will modify configuration parameters (also known as knobs) related to Stage 0 in the `core64.yaml` parameter file. These changes will then be evaluated using standard RISC-V benchmarks, CoreMark, and Shakti-provided microbenchmarks.  

---

**Phase 1: Baseline Setup and Evaluation**  

**1. Identify Stage 0 Parameters**  
Locate parameters related to PCGen and Branch Predictor in the `core64.yaml` file. These serve as the initial targets for performance tuning.  

**2. Build the Baseline Core**  
Using the default configuration from `core64.yaml`, build the C-Class core to establish a reference point for comparison.  

---

**Future Works**  

Run the built core in the Bluespec Verilog simulation environment. Execute RISC-V benchmarks, CoreMark, and Shakti-provided microbenchmarks to gather baseline performance metrics.  
