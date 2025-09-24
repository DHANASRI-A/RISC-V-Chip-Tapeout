

## Yosys Synthesis Process

![illustration](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/6c12526dcb85ed88990b0dc39877de689cab2a79/Week_1/Day_1/Pictures/Synthesis(Illustration).png)

### **Step 1: List of Commands**

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

### **Step 2: Command Outputs / Screenshots**

**1. Set directory to Verilog files**

```bash
cd verilog_files
```

*(Attach screenshot here showing the terminal in `verilog_files` folder)*

---

**2. Read standard cell library**

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

*(Attach screenshot here showing library read successfully)*

---

**3. Read RTL design**

```bash
read_verilog good_mux.v
```

*(Attach screenshot here showing RTL read successfully)*

---

**4. Synthesize design**

```bash
synth -top good_mux
```

*(Attach screenshot here showing synthesis progress/output)*

---

**5. Technology mapping using ABC**

```bash
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

*(Attach screenshot here showing mapping completed)*

---

**6. Visualize synthesized netlist**

```bash
show
```

*(Attach screenshot of synthesized logic diagram)*

---

**7. Write synthesized netlist**

```bash
write_verilog good_mux_netlist.v
```

*(Attach screenshot showing file creation)*

---

**8. Open synthesized netlist in Mousepad**

```bash
!mousepad good_mux_netlist.v
```

*(Attach screenshot of netlist opened in Mousepad)*

---

**9. Write netlist without attributes**

```bash
write_verilog -noattr good_mux_netlist.v
```

*(Attach screenshot showing simplified netlist creation)*

---

**10. Open simplified netlist in Mousepad**

```bash
!mousepad good_mux_netlist.v
```

*(Attach screenshot of simplified netlist opened in Mousepad)*

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

If you want, I can **now also combine this with the iVerilog & GTKWave section** in the same style (commands first, screenshots after, table for explanation) so the README is fully professional and ready for Week 0.

Do you want me to do that?
