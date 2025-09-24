

## Yosys Synthesis Process

![illustration](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/6c12526dcb85ed88990b0dc39877de689cab2a79/Week_1/Day_1/Pictures/Synthesis(Illustration).png)

### Step 1: List of Commands 

```bash
cd verilog_files
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog good_mux_netlist.v
!mousepad good_mux_netlist.v
write_verilog -noattr good_mux_netlist.v
!mousepad good_mux_netlist.v
```

> **Note:** First, we list all the commands together for clarity.

---

### **Step 2: Command Outputs**

**1. Set directory to Verilog files**

```bash
cd verilog_files
```


---

**2. Read standard cell library**

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```



---

**3. Read RTL design**

```bash
read_verilog good_mux.v
```



---

**4. Synthesize design**

```bash
synth -top good_mux
```

![image](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/577fca01537cc44cd156572ac023599945fad318/Week_1/Day_1/Pictures/commands.png)

---

**5. Technology mapping using ABC**

```bash
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```



---

**6. Visualize synthesized netlist**

```bash
show
```

![output](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/9bea3660bc82807dc22347a3e322bf62ad4490c1/Week_1/Day_1/Pictures/Synthesis%20Output.png)

---

**7. Write synthesized netlist**

```bash
write_verilog good_mux_netlist.v
```



---

**8. Open synthesized netlist in Mousepad**

```bash
!mousepad good_mux_netlist.v
```

![image](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/9bea3660bc82807dc22347a3e322bf62ad4490c1/Week_1/Day_1/Pictures/show.png)

---

**9. Write netlist without attributes**

```bash
write_verilog -noattr good_mux_netlist.v
```



---

**10. Open simplified netlist in Mousepad**

```bash
!mousepad good_mux_netlist.v
```



---

### **Step 3: Command Explanations**

| Step | Command                                                      | Purpose                                                     |
| ---- | ------------------------------------------------------------ | ----------------------------------------------------------- |
| 1    | `cd verilog_files`                                           | Navigate to the folder containing our Verilog RTL files.    |
| 2    | `read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib` | Load the standard cell library for synthesis.               |
| 3    | `read_verilog good_mux.v`                                    | Read our RTL design into Yosys.                             |
| 4    | `synth -top good_mux`                                        | Perform RTL-to-gate synthesis on the top module `good_mux`. |
| 5    | `abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`      | Optimize and map the design to technology-specific gates.   |
| 6    | `show`                                                       | Display a graphical view of the synthesized netlist.        |
| 7    | `write_verilog good_mux_netlist.v`                           | Save the synthesized netlist with attributes.               |
| 8    | `!mousepad good_mux_netlist.v`                               | Open the netlist in Mousepad for inspection.                |
| 9    | `write_verilog -noattr good_mux_netlist.v`                   | Save the netlist without synthesis attributes.              |
| 10   | `!mousepad good_mux_netlist.v`                               | Open the simplified netlist in Mousepad.                    |

---

### **Step 4: Synthesis Flow Summary**

```
RTL Design (good_mux.v)
        │
        ▼
Read Standard Cell Library
        │
        ▼
RTL → Yosys Synthesis (synth -top)
        │
        ▼
Technology Mapping (ABC)
        │
        ▼
Synthesized Netlist (good_mux_netlist.v)
        │
        ▼
Verification / Inspection (Mousepad)
```

> **Note:** The **primary inputs and outputs** remain the same between RTL and netlist, so the same testbench can be used.

---


