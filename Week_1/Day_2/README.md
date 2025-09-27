# Digital Synthesis with Yosys

## Table of Contents
1. [Understanding the Library](#understanding-the-library)  
2. [PVT Corners](#pvt-corners)
3. [Types of Synthesis](#types-of-synthesis) 
   - [Hierarchical Synthesis](#hierarchical-synthesis)  
   - [Flat Synthesis](#flat-synthesis)
4. [Submodule-Level Synthesis](#submodule-level-synthesis)

---

## Understanding the Library

The standard cell library used in synthesis is:  

```

sky130_fd_sc_hd__tt_025C_1v80.lib

```

### Breakdown of the Name
- **sky130_fd_sc_hd** → SkyWater 130nm, standard cell, high-density variant.  
- **tt** → *Typical process corner* (average transistor behavior).  
- **025C** → Characterized at **25°C** operating temperature.  
- **1v80** → Characterized at **1.80 V** supply voltage.  

This `.lib` file provides detailed information about each cell such as:  
- Logic functionality (e.g., AND, OR, Flip-Flop).  
- Timing characteristics (delays, setup/hold times).  
- Power consumption.  
- Area footprint.  

During synthesis, Yosys maps the RTL design onto these cells to create a **gate-level netlist** that reflects real hardware.

---

## PVT Corners

**PVT = Process, Voltage, Temperature** — the three key parameters that affect circuit behavior:  

- **Process (P):** Accounts for manufacturing variations (fast, typical, slow transistors).  
- **Voltage (V):** Supply voltage fluctuations (e.g., 1.62 V – 1.98 V around 1.80 V nominal).  
- **Temperature (T):** Operating range from low (e.g., -40°C) to high (e.g., 125°C).  

Designs must be verified across multiple PVT corners to ensure they meet **timing, power, and reliability requirements** under all real-world conditions.  

### Example PVT Corners

| Corner | Process | Voltage | Temperature | Usage |
|--------|---------|---------|-------------|-------|
| **FF** | Fast    | 1.98 V  | -40°C       | Best-case delay (fastest) |
| **TT** | Typical | 1.80 V  | 25°C        | Nominal operating point |
| **SS** | Slow    | 1.62 V  | 125°C       | Worst-case delay (slowest) |

---



## Types of Synthesis
There are two types of synthesis approaches in Yosys:
1. **Hierarchical Synthesis**  
2. **Flat Synthesis**

---

## Hierarchical Synthesis
**Explanation:**
- Preserves the module hierarchy (each sub-module remains separate).  
- Makes debugging and analysis easier.  
- Useful for modular design and incremental development.  
- Optimizations happen within each module independently.  

### Commands:
```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v 
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_modules
write_verilog multiple_modules_hier.v 
write_verilog -noattr multiple_modules_hier.v 
````

![hier](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/57c5946156859e6f9861df0e7054104bfd237163/Week_1/Day_2/Pictures/Hier.png)

---

## Flat Synthesis

**Explanation:**

* Flattens the hierarchy into a single-level netlist.
* Provides better **global optimization** across modules.
* Less readable, but efficient for final design stages.
* Often used before tape-out for best performance.

### Commands:

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
flatten
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog multiple_modules_flat.v
write_verilog -noattr multiple_modules_flat.v
```

![flatten](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/57c5946156859e6f9861df0e7054104bfd237163/Week_1/Day_2/Pictures/Flatten.png)

---




## Submodule-Level Synthesis

Sometimes, instead of synthesizing the full top-level design, we may want to synthesize individual **submodules**.  
This approach is useful when:  

- The **top-level design is very large**, making full synthesis slow or resource-heavy.  
- A **submodule is instantiated multiple times** in the top design. Synthesizing it once and reusing its netlist avoids redundant work.  
- It allows for **modular development and debugging**, since each block can be verified independently.  
- Later, all synthesized submodule netlists can be combined to form the complete design.  

---

### Example: Synthesizing `sub_module1`

**Commands:**
```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top sub_module1
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show sub_module1
write_verilog sub_module1_netlist.v
write_verilog -noattr sub_module1_netlist.v
````




