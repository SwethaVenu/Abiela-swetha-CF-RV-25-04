<img width="1250" height="581" alt="makefile1" src="https://github.com/user-attachments/assets/b484af27-5851-4fab-aca3-fba231eaf142" />
![rtl1](https://github.com/user-attachments/assets/4c50d067-383d-4930-8b43-7bdb3bc60b24)
**execution results from `rtl1.dump` before modifying `core64.yaml`**

---

### Execution Results (Before `core64.yaml` Modification)

```
Retired Instructions: 0x00000000c6991e = 13,011,486  
Cycles:               0x00000000f712cc = 16,167,244  
Branch Mispredictions: 0x0000000083658f = 8,622,991  
Number of Jumps:       0x00000000024f07 = 151,559  
Number of Branches:    0x0000000020c392 = 2,147,474  
MUL/DIV Instructions:  0x0000000005bcd8 = 375,896
```

---

### 📄 Sample Trace from `rtl1.dump`

```
core 0: 3 0x000000008000454a (0xc0202573) x10 0x00000000c6991e
core 0: 3 0x000000008000454e (0xc0802573) x10 0x00000000f712cc
core 0: 3 0x0000000080004552 (0xb0c02573) x10 0x0000000083658f
core 0: 3 0x0000000080004556 (0xb0402573) x10 0x00000000024f07
core 0: 3 0x000000008000455a (0x8b052573) x10 0x0000000020c392
core 0: 3 0x000000008000455e (0x8c0802573) x10 0x0000000005bcd8
core 0: 3 0x0000000080004562 (0x00002537) x10 0x00000000002000
```

---

