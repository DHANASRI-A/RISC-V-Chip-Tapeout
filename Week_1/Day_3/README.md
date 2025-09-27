

# Logic Optimization with Yosys

This README demonstrates the use of **`opt_clean -purge`** in Yosys for different types of logic optimization:

1. Combinational Logic Optimization
2. Sequential Logic Optimization
3. Unused Logic Optimization

---

## Table of Contents

* [Combinational Logic Optimization](#combinational-logic-optimization)
* [Sequential Logic Optimization](#sequential-logic-optimization)
* [Unused Logic Optimization](#unused-logic-optimization)
* [Optimization Flow](#optimization-flow)
* [Summary Table](#summary-table)

---

## Combinational Logic Optimization

**Code:** `opt_check.v`

```verilog
module opt_check (input a , input b , output y);
    assign y = a?b:0;
endmodule
```

**Purpose:**
Demonstrates combinational logic that can be simplified by removing redundant expressions.

**Yosys Commands:**

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_check
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog opt_check_before_purge.v  # Netlist before purge
opt_clean -purge                        # Remove unused/redundant logic
write_verilog opt_check_after_purge.v   # Netlist after purge
show
```

![opt\_check](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/db085dd141e22a304d37adfb0bac71a97d757c9d/Week_1/Day_3/Pictures/opt_check.png)

**Explanation of `opt_clean -purge`:**

* Removes **unused logic** and redundant connections.
* Optimizes the netlist by **purging dead gates**.
* Reduces area and simplifies design.

---

## Sequential Logic Optimization

**Code:** `dff_const1.v`

```verilog
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
    if(reset)
        q <= 1'b0;
    else
        q <= 1'b1;
end
endmodule
```

**Purpose:**
Simplifies sequential logic with constant assignments.

**Yosys Commands:**

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog dff_const1_before_purge.v
opt_clean -purge
write_verilog dff_const1_after_purge.v
show
```

![dff\_const1](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/db085dd141e22a304d37adfb0bac71a97d757c9d/Week_1/Day_3/Pictures/dff_const1.png)

---

## Unused Logic Optimization

**Code:** `counter_opt.v`

```verilog
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];

always @(posedge clk ,posedge reset)
begin
    if(reset)
        count <= 3'b000;
    else
        count <= count + 1;
end
endmodule
```

**Purpose:**
Removes unused logic (higher bits of the counter not driving outputs).

**Yosys Commands:**

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt.v
synth -top counter_opt
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog counter_opt_before_purge.v
opt_clean -purge
write_verilog counter_opt_after_purge.v
show
```

### Before `opt_clean -purge`

![before](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/db085dd141e22a304d37adfb0bac71a97d757c9d/Week_1/Day_3/Pictures/before.png)

### After `opt_clean -purge`

![after](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/db085dd141e22a304d37adfb0bac71a97d757c9d/Week_1/Day_3/Pictures/after.png)

---

## Optimization Flow

```
RTL Code → Synthesis → Netlist → opt_clean -purge → Optimized Netlist
```


---

## Summary Table

| Optimization Type | Example Module | What is Optimized                  |
| ----------------- | -------------- | ---------------------------------- |
| Combinational     | `opt_check`    | Redundant expressions, logic gates |
| Sequential        | `dff_const1`   | Constant assignments, DFF behavior |
| Unused Logic      | `counter_opt`  | Wires/reg not driving outputs      |

---

**Conclusion:**

* **`opt_clean -purge`** is essential for cleaning up synthesized netlists.
* Removes **unused or redundant logic**, reduces area, and simplifies designs.
* Applying this command makes designs more **efficient and professional** before technology mapping or further optimizations.

---

