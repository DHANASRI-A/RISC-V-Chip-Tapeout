
## **Day 1: Introduction to Icarus Verilog (iverilog)**

**Objective:** Understand how Icarus Verilog is used for RTL simulation and the role of testbenches.

### **Key Concepts:**

#### **1. Simulator**

* A simulator is a tool used to **verify and check a design**.
* In this program, the simulator used is **Icarus Verilog (`iverilog`)**.

#### **2. Testbench**

* A testbench is used to **ensure that the design obeys all specifications**.
* It provides stimulus to the design and observes outputs.
* **Important:** Testbenches **do not have primary inputs or primary outputs**, unlike the actual design.

![Week_1/Day_1/Pictures/Testbench.png](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/ff26b68227088b1c3e72e9c1073c8cd009f19890/Week_1/Day_1/Pictures/Testbench.png)

#### **3. How Simulation Works**

* The simulator continuously **monitors changes in the inputs**.
* If an input changes, the **output is re-evaluated**.
* If there is **no change in the input, the output remains unchanged**.

![image](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/ff26b68227088b1c3e72e9c1073c8cd009f19890/Week_1/Day_1/Pictures/Iverilog.png)

### **Notes:**  
> - **A design may have one or more primary inputs or outputs.**  
> - **A testbench does not have primary inputs or outputs.**




#### **Basic Simulation Workflow**

```bash
# Compile Verilog design and testbench
iverilog -o output_file design.v testbench.v

# Run simulation to generate VCD file
vvp output_file

# View waveform in GTKWave
gtkwave output_file.vcd
```

> 💡 The **VCD file (`.vcd`)** records all signal changes over time and can be visualized using GTKWave to verify RTL functionality before synthesis.

---

