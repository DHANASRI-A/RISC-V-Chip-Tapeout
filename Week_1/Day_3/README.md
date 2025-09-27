
# Logic Optimization in Verilog

This document demonstrates **logic optimization** techniques in digital design using Yosys. Logic optimization helps reduce area, power, and improve performance.

---

## Table of Contents

- [Combinational Logic Optimization](#combinational-logic-optimization)  
- [Sequential Logic Optimization](#sequential-logic-optimization)  
- [Unused Logic Optimization](#unused-logic-optimization)  
- [Effect of `opt_clean -purge`](#effect-of-opt_clean--purge)  

---

## Combinational Logic Optimization

**Example Module:**

```verilog
module opt_check (input a , input b , output y);
    assign y = a ? b : 0;
endmodule
````

**Yosys Commands:**

```bash
yosys
read_verilog opt_check.v
synth -top opt_check
opt
show
write_verilog opt_check_before_purge.v

opt_clean -purge
show
write_verilog opt_check_after_purge.v
```

* `opt` → Simplifies Boolean expressions.
* `opt_clean -purge` → Removes **unused wires or redundant logic**, producing a leaner netlist.

**Observation:**

* **Before `opt_clean -purge`**: Intermediate wires that are not needed remain in the netlist.
* **After `opt_clean -purge`**: All redundant or unused wires are removed, reducing area and complexity.

**Waveform:**
(Include screenshot here after simulation)

---

## Sequential Logic Optimization

**Example Module:**

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

**Yosys Commands:**

```bash
yosys
read_verilog dff_const1.v
synth -top dff_const1
opt
show
write_verilog dff_const1_before_purge.v

opt_clean -purge
show
write_verilog dff_const1_after_purge.v
```

**Observation:**

* Constant sequential outputs are simplified.
* Any redundant DFF logic or unnecessary signals are removed after `opt_clean -purge`.

**Waveform:**
(Include screenshot here after simulation)

---

## Unused Logic Optimization

**Example Module:**

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

**Yosys Commands:**

```bash
yosys
read_verilog counter_opt.v
synth -top counter_opt
opt
show
write_verilog counter_opt_before_purge.v

opt_clean -purge
show
write_verilog counter_opt_after_purge.v
```

**Observation:**

* Bits `count[2:1]` are **unused** in output `q`.
* **Before `opt_clean -purge`**: All bits remain in the netlist.
* **After `opt_clean -purge`**: Only the necessary bit `count[0]` is retained, reducing area and simplifying logic.

**Waveform:**
(Include screenshot here after simulation)

---

## Effect of `opt_clean -purge`

* **Without `opt_clean -purge`**: Netlist contains unused wires, redundant logic, and intermediate signals.
* **With `opt_clean -purge`**: Netlist is cleaner, smaller, and optimized for further synthesis or mapping.
* **Recommendation:** Always run `opt_clean -purge` after `opt` to generate an efficient netlist.
This ensures **lean, optimized designs**, whether for combinational logic, sequential logic, or unused logic.

---



