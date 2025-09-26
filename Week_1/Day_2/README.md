# Digital Synthesis with Yosys

## Table of Contents
1. [Understanding the Library](#understanding-the-library)  
2. [PVT Corners](#pvt-corners)  
3. [Types of Synthesis](#types-of-synthesis)  
   - [Hierarchical Synthesis](#hierarchical-synthesis)  
   - [Flat Synthesis](#flat-synthesis)  
4. [Comparison: Hierarchical vs Flat Synthesis](#comparison-hierarchical-vs-flat-synthesis)  

---

## Understanding the Library
The library name used is:

```

sky130_fd_sc_hd__tt_025C_1v80.lib

````

**Breakdown of the name:**
- **tt** → Typical corner  
- **025C** → Temperature (25°C)  
- **1v80** → Voltage (1.80 V)  

This library contains information about **cell timing, power, and functionality**.  
It is crucial for mapping RTL code to actual hardware cells.

---

## PVT Corners
**PVT stands for:**
- **P** → Process  
- **V** → Voltage  
- **T** → Temperature  

These three parameters define how the design behaves under different manufacturing and environmental conditions.  
They are very important for ensuring that the design works in all possible scenarios.

---

## Types of Synthesis
There are two types of synthesis approaches in Yosys:
1. **Hierarchical Synthesis**  
2. **Flat Synthesis**

---

## Hierarchical Synthesis
In this method, the design hierarchy (**modules inside modules**) is preserved during synthesis.  

### Commands:
```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v 
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_modules   # (screenshot can be added after this step)
write_verilog multiple_modules_hier.v 
!mousepad multiple_modules_hier.v 
write_verilog -noattr multiple_modules_hier.v 
!mousepad multiple_modules_hier.v 
````

*(Add screenshot after this block)*

---

## Flat Synthesis

In this method, the hierarchy is **flattened** — meaning all sub-modules are merged into a single-level netlist.

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
!mousepad multiple_modules_flat.v
```

*(Add screenshot after this block)*

---

## Comparison: Hierarchical vs Flat Synthesis

| Feature              | Hierarchical Synthesis                | Flat Synthesis                              |
| -------------------- | ------------------------------------- | ------------------------------------------- |
| **Hierarchy**        | Preserves module hierarchy            | Flattens all modules into one level         |
| **Readability**      | Easier to debug and analyze           | More difficult to read/debug                |
| **Optimization**     | Optimized within modules only         | Better global optimization possible         |
| **Compilation Time** | Faster (smaller sub-problems)         | Slower (entire design optimized together)   |
| **Use Case**         | Good for modular design and debugging | Good for final optimization before tape-out |

---


