

# Gate-Level Simulation (GLS) and Simulation-Synthesis Mismatches

This README demonstrates **Gate-Level Simulation (GLS)** using Yosys, Icarus Verilog, and GTKWave, along with examples of **simulation-synthesis mismatches** caused by coding practices.

---

## Table of Contents

* [GLS: Ternary Operator MUX](#gls-ternary-operator-mux)
* [Simulation-Synthesis Mismatch: Non-Clocked `always`](#simulation-synthesis-mismatch-non-clocked-always)
* [Simulation-Synthesis Mismatch: Blocking Assignment](#simulation-synthesis-mismatch-blocking-assignment)

---

## Flow: RTL → Synthesis → GLS → Compare Waveforms

```text
        RTL Code (Verilog)
               │
               ▼
        RTL Simulation
               │
               ▼
          Synthesis
               │
               ▼
        Gate-Level Netlist
               │
               ▼
     Gate-Level Simulation (GLS)
               │
               ▼
   Compare RTL and GLS Waveforms
```

**Explanation:**

* RTL simulation verifies logic correctness.
* Synthesis converts RTL into a **gate-level netlist** using standard library cells.
* GLS verifies the **post-synthesis behavior** matches RTL.
* Waveforms are compared to detect mismatches.

---

## GLS: Ternary Operator MUX

**Verilog Code:** `ternary_operator_mux.v`

```verilog
module ternary_operator_mux (
    input i0, 
    input i1, 
    input sel, 
    output y
);
assign y = sel ? i1 : i0;
endmodule
```

**Purpose:**

* Shows clean combinational logic using a ternary operator.
* Good example for comparing RTL simulation and gate-level simulation.

**Synthesis Commands:**

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr ternary_operator_mux_net.v
```

**Gate-Level Simulation Commands:**

```bash
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
```

**Explanation of `iverilog` command:**

* `../my_lib/verilog_model/primitives.v` → contains basic Verilog primitives like AND, OR gates.
* `../my_lib/verilog_model/sky130_fd_sc_hd.v` → models the standard cells from the SkyWater library.
* `ternary_operator_mux_net.v` → synthesized netlist from Yosys.
* `tb_ternary_operator_mux.v` → testbench to verify functionality.

```bash
./a.out
gtkwave tb_ternary_operator_mux.vcd
```

**Screenshots:**

*  RTL waveform
  
![rtl](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/8aaea6ef6832cfa4261003e23dbc6d0b7e3909ab/Week_1/Day_4/Pictures/1.RTL%20waveform.png)
*  GLS waveform
  
![gls](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/8aaea6ef6832cfa4261003e23dbc6d0b7e3909ab/Week_1/Day_4/Pictures/1.GLS%20waveform.png)

---

## Simulation-Synthesis Mismatch: Non-Clocked `always`

**Verilog Code:** `bad_mux.v`

```verilog
module bad_mux (
    input i0, 
    input i1, 
    input sel, 
    output reg y
);
always @(sel)
begin
    if(sel)
        y <= i1;
    else 
        y <= i0;
end
endmodule
```

**Explanation:**

* `always @(sel)` is sensitive only to `sel` instead of `*` or a proper clock.
* This can produce **different behavior after synthesis**, causing simulation-synthesis mismatch.

**RTL Simulation Commands:**

```bash
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

**Synthesis Commands:**

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog bad_mux.v
synth -top bad_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr bad_mux_net.v
```

**Gate-Level Simulation Commands:**

```bash
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

**Screenshots:**

* RTL waveform
![rtl](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/b0d0648f0bbc60993b826cda7fb935d4f065f8af/Week_1/Day_4/Pictures/RTL%20bad_mux.png)
* GLS waveform 
![gls](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/b0d0648f0bbc60993b826cda7fb935d4f065f8af/Week_1/Day_4/Pictures/GLS%20bad_mux.png)

---

## Simulation-Synthesis Mismatch: Blocking Assignment

**Verilog Code:** `blocking_caveat.v`

```verilog
module blocking_caveat (
    input a, 
    input b, 
    input c, 
    output reg d
);
reg x;
always @(*)
begin
    d = x & c;   // blocking assignment
    x = a | b;   // blocking assignment
end
endmodule
```

**Explanation:**

* Blocking assignments (`=`) in combinational logic may cause **simulation-synthesis mismatches**.
* Simulation evaluates statements **sequentially**, but synthesis tools may optimize differently, producing unexpected results.

**RTL Simulation Commands:**

```bash
iverilog blocking_caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```

**Synthesis Commands:**

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr blocking_caveat_net.v
```

**Gate-Level Simulation Commands:**

```bash
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```

**Screenshots:**

* Add RTL waveform here
* Add GLS waveform here

---

**Conclusion:**

* GLS verifies **post-synthesis behavior** matches RTL.
* Mismatches are caused by:

  1. Non-clocked or incomplete sensitivity lists.
  2. Blocking assignments in combinational logic.
  3. Improper coding styles.
* Correct RTL coding practices ensure **consistent RTL and GLS behavior**.

---
