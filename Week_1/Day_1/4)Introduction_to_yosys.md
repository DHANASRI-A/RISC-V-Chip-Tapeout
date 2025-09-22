
## Yosys – RTL to Netlist

### 1. **What is a Synthesizer?**

* A **synthesizer** is a tool that converts **RTL (Register Transfer Level) design** into a **gate-level netlist**.
* In this program, the synthesizer used is **Yosys**.

---

### 2. **Netlist File**

* The **netlist** is the **gate-level representation** of our RTL design.
* It contains **logic gates, connections, and primary inputs/outputs**, but not high-level RTL constructs like `always` blocks or `if` statements.
* This file is the **output of Yosys**.

![yosys](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/b07d02d8e02695935208c37325499bfccc3935b9/Week_1/Day_1/Pictures/Yosys.png)

---

### 3. **Verifying the Netlist**

* The generated netlist can be **simulated using iVerilog** to verify that synthesis preserved the design behavior.
* The **same testbench** used for the RTL design can be applied to the netlist.

![synthesis](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/b07d02d8e02695935208c37325499bfccc3935b9/Week_1/Day_1/Pictures/synthesis%20verification.png)

---

### 4. **Important Note**

> The set of **primary inputs and primary outputs remain the same** between the RTL design and synthesized netlist.
> Therefore, **the same testbench can be used** for both RTL and netlist verification.

---


