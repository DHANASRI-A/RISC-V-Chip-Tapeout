

## Yosys Synthesis Process

### **Commands**

```bash
# 1. Set the directory to the Verilog files
cd verilog_files

# 2. Read the standard cell library
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# 3. Read the Verilog RTL design
read_verilog good_mux.v

# 4. Synthesize the design (specify top module)
synth -top good_mux

# 5. Run technology mapping using ABC with the library
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# 6. Visualize the synthesized netlist
show

# 7. Write the synthesized netlist to a Verilog file
write_verilog good_mux_netlist.v

# 8. Open the netlist in a text editor
!mousepad good_mux_netlist.v

# 9. Write the netlist without attributes
write_verilog -noattr good_mux_netlist.v

# 10. Open the no-attribute netlist in a text editor
!mousepad good_mux_netlist.v
```

---

### **Explanation of Each Command**

| Step | Command                                                      | Purpose                                                                                              |
| ---- | ------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------- |
| 1    | `cd verilog_files`                                           | Navigate to the folder containing our Verilog RTL files.                                             |
| 2    | `read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib` | Load the standard cell library for synthesis. This defines the technology and available logic gates. |
| 3    | `read_verilog good_mux.v`                                    | Read our RTL design into Yosys.                                                                      |
| 4    | `synth -top good_mux`                                        | Perform RTL-to-gate synthesis on the top module `good_mux`.                                          |
| 5    | `abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`      | Optimize and map the design to technology-specific gates from the standard cell library.             |
| 6    | `show`                                                       | Display a graphical view of the synthesized netlist (logic gates and connections).                   |
| 7    | `write_verilog good_mux_netlist.v`                           | Save the synthesized netlist to a Verilog file including all attributes.                             |
| 8    | `!mousepad good_mux_netlist.v`                               | Open the synthesized netlist in Mousepad to inspect its structure.                                   |
| 9    | `write_verilog -noattr good_mux_netlist.v`                   | Save the netlist **without synthesis attributes**, making it simpler and cleaner.                    |
| 10   | `!mousepad good_mux_netlist.v`                               | Open the simplified netlist in Mousepad for inspection.                                              |

---

### **Summary of the Synthesis Flow**

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

> **Notes:**
>
> * The netlist preserves the **same primary inputs and outputs** as the RTL, so the same testbench can be used for verification.
> * Viewing the netlist helps understand how the RTL constructs are translated into **gates and connections**.

---

