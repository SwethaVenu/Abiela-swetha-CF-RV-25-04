## Branch Predictor Modification in `core64.yaml`

We modified the branch predictor parameters in the `core64.yaml` file as part of Stage 0 analysis.  
Specifically, we changed the **Branch History Table Depth** (`bht_depth`) from **512** to **500** to observe its impact.

### Modified Section:
```yaml
branch_predictor:
  instantiate: True
  predictor: gshare
  btb_depth: 32
  bht_depth: 500  # Modified from 512
  history_len: 8
  history_bits: 5
  ras_depth: 8
````

###  Results from `rtl1.dump` and `app_log`:

| Metric                | CSR Value (Hex)     | Decimal Value |
| --------------------- | ------------------- | ------------- |
| Retired Instructions  | 0x00000000000011000 | 69,632        |
| Cycles                | 0x00000000000011300 | 70,400        |
| Branch Mispredictions | 0x000000000001130c  | 70,412        |
| Number of Jumps       | 0x0000000000000001  | 1             |
| MUL/DIV Instructions  | 0x000000000001130c  | 70,412        |
 *\This change helps evaluate the sensitivity of prediction accuracy and execution metrics to history table depth.*

```
![mod1](https://github.com/user-attachments/assets/4798decc-830c-4993-8e61-182e6fdc7845)
![app_log1](https://github.com/user-attachments/assets/e2248d3d-73e3-4113-aa44-0fcc82002ee2)
<img width="909" height="142" alt="rtl2mo" src="https://github.com/user-attachments/assets/845013da-fc39-4df6-980a-006bc0259cbe" />


