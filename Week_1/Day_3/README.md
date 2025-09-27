
# Logic Optimization with Yosys

This README demonstrates the use of **`opt_clean -purge`** in Yosys for different types of logic optimization:  

1. Combinational Logic Optimization  
2. Sequential Logic Optimization  
3. Unused Logic Optimization  

We focus on observing the difference **before and after** applying `opt_clean -purge`.

---

## Table of Contents

- [Combinational Logic Optimization](#combinational-logic-optimization)
- [Sequential Logic Optimization](#sequential-logic-optimization)
- [Unused Logic Optimization](#unused-logic-optimization)

---

## Combinational Logic Optimization

**Code:** `opt_check.v`

```verilog
module opt_check (input a , input b , output y);
    assign y = a?b:0;
endmodule
````

**Purpose:**
This module demonstrates combinational logic that can be simplified by removing redundant expressions.

**Yosys Commands:**

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_check
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog opt_check_before_purge.v  # Netlist before purge
opt_clean -purge                        # Remove unused logic
write_verilog opt_check_after_purge.v   # Netlist after purge
show
```

**Explanation of `opt_clean -purge`:**

* Removes **unused logic** and redundant connections.
* Optimizes the netlist by **purging dead gates** that do not affect outputs.
* Helps reduce area and simplify the design.

### Before `opt_clean -purge`

![opt\_check before purge](Images/opt_check_before.png)

### After `opt_clean -purge`

![opt\_check after purge](Images/opt_check_after.png)

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
Demonstrates sequential logic optimization where constant assignments can be simplified.

**Yosys Commands:**

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog dff_const1_before_purge.v
opt_clean -purge
write_verilog dff_const1_after_purge.v
show
```

### Before `opt_clean -purge`

![dff\_const1 before purge](Images/dff_const1_before.png)

### After `opt_clean -purge`

![dff\_const1 after purge](Images/dff_const1_after.png)

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
Shows how unused logic (like higher bits of the counter that are not connected to outputs) can be removed.

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

![counter]([Images/counter_opt_before.png](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/178bc30c4eb7211e4dd9d14af0911505e7c76e53/Week_1/Day_3/Pictures/before.png))

### After `opt_clean -purge`

![counter]([Images/counter_opt_after.png](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/178bc30c4eb7211e4dd9d14af0911505e7c76e53/Week_1/Day_3/Pictures/after.png))

---

**Conclusion:**

* The **`opt_clean -purge`** command is essential for cleaning up the netlist after synthesis.
* It removes **unused or redundant logic**, reduces area, and simplifies designs.
* Applying this command makes designs more **efficient and professional** before technology mapping or further optimizations.


---

