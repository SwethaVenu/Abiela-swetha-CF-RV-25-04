## Week 2 Update – C-Class Core Performance Analysis (Stage 0: PCGen & Branch Predictor)

- Installed Bluespec Verilog and set up the environment successfully.
- Attempted to build the C-Class core using the default `core64.yaml` configuration. Encountered errors during simulation; debugging is in progress.
- Identified the following key Stage 0 parameters in `core64.yaml`:

### Stage 0 Parameters Identified:

1. reset_pc – Sets the initial value of the Program Counter.
2. branch_predictor.instantiate – Enables/disables the branch predictor unit.
3. branch_predictor.predictor – Type of branch predictor used (e.g., `gshare`).
4. branch_predictor.btb_depth – Size of the Branch Target Buffer.
5. branch_predictor.bht_depth – Size of the Branch History Table.
6. branch_predictor.history_len – Global history length used by the predictor.
7. branch_predictor.history_bits – Number of bits used for indexing into the BHT.
8. branch_predictor.ras_depth – Depth of the Return Address Stack.

- We are planning parameter variations (e.g., increasing `btb_depth`, reducing `history_len`) to study their impact on performance.

