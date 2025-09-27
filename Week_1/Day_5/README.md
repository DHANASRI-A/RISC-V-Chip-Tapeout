

# README – If/Case Constructs and Looping Constructs in Verilog

## Table of Contents

1. [Introduction](#introduction)
2. [If vs Case](#if-vs-case)
3. [Inferred Latches](#inferred-latches)
   * [Incomplete If Example](#incomplete-if-example)
   * [Incomplete Case Example](#incomplete-case-example)
   * [Partial Case Assignment](#partial-case-assignment)
   * [Overlapping Case Example](#overlapping-case-example)
4. [Looping Constructs](#looping-constructs)
   * [Difference between For Loop and Generate For Loop](#difference-between-for-loop-and-generate-for-loop)
   * [4:1 Mux using Case](#41-mux-using-case)
   * [For Loop in Mux](#for-loop-in-mux)
   * [Generate For Loop – Ripple Carry Adder](#generate-for-loop--ripple-carry-adder)
5. [Key Takeaways](#key-takeaways)

---

## Introduction

In Verilog, **conditional constructs** (`if` and `case`) and **looping constructs** (`for` and `generate-for`) are essential for building digital circuits efficiently.

* If/Case statements decide the flow of signals based on conditions.
* Loops simplify writing repetitive logic and make RTL designs scalable.

However, **improper usage** of these constructs can cause problems such as:

* Inferred latches (unwanted memory elements in combinational logic).
* Ambiguous or overlapping logic in `case` statements.

This document explores **pitfalls and solutions** with clear examples.

---

## If vs Case

* **`if` statement** → works on **priority checking**. Used when conditions are checked in sequence (e.g., **priority encoders**).
* **`case` statement** → works on **parallel checking**. Used when multiple conditions are evaluated independently (e.g., **multiplexers**).

**Problem**: Both constructs can cause *inferred latches* if all conditions are not covered.

---

## Inferred Latches

### Incomplete If Example

```verilog
module incomp_if (input i0 , input i1 , input i2 , output reg y);
always @ (*)
begin
    if(i0)
        y <= i1;
end
endmodule
```

Since `y` is not assigned when `i0 = 0`, synthesis tool inserts a **latch**.

*Screenshot here: Yosys/GTKWave showing latch inference*

---

### Incomplete Case Example

```verilog
module incomp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
always @ (*)
begin
    case(sel)
        2'b00 : y = i0;
        2'b01 : y = i1;
    endcase
end
endmodule
```

No `default` branch → `y` retains old value when `sel=2’b10` or `2’b11`.
Hence, latch is inferred.

*Screenshot here: synthesis result with latch*

---

### Partial Case Assignment

```verilog
module partial_case_assign (input i0 , input i1 , input i2 , input [1:0] sel, output reg y , output reg x);
always @ (*)
begin
    case(sel)
        2'b00 : begin
            y = i0;
            x = i2;
        end
        2'b01 : y = i1;  // x missing
        default : begin
            x = i1;
            y = i2;
        end
    endcase
end
endmodule
```

In branch `2'b01`, `x` is not updated → **latch inferred for `x`**.

*Screenshot here: waveform mismatch*

---

### Overlapping Case Example

```verilog
module bad_case (input i0 , input i1, input i2, input i3 , input [1:0] sel, output reg y);
always @(*)
begin
    case(sel)
        2'b00: y = i0;
        2'b01: y = i1;
        2'b10: y = i2;
        2'b1?: y = i3;   // Overlaps with 2’b10 and 2’b11
    endcase
end
endmodule
```

`2’b10` and `2’b1?` overlap. Different synthesis tools may optimize differently → mismatch risk.

*Screenshot here: Yosys schematic with overlapping logic*

---

## Looping Constructs

In Verilog, loops help reduce repetitive coding.

### Difference between For Loop and Generate For Loop

| Feature              | `for` Loop (inside always block)                | `generate for` Loop (outside always block)           |
| -------------------- | ----------------------------------------------- | ---------------------------------------------------- |
| Usage                | For evaluating expressions in procedural blocks | For instantiating hardware multiple times            |
| Location             | Inside `always` or `initial` block              | Outside procedural blocks                            |
| Example Applications | Writing scalable multiplexers, encoders         | Creating repeated hardware (adders, shift registers) |
| Executes At          | Simulation/runtime                              | Elaboration/synthesis time                           |

---

### 4:1 Mux using Case

```verilog
module mux_4to1_case (
    input [3:0] data_in,
    input [1:0] sel,
    output reg out
);
always @(*) begin
    case (sel)
        2'b00: out = data_in[0];
        2'b01: out = data_in[1];
        2'b10: out = data_in[2];
        2'b11: out = data_in[3];
    endcase
end
endmodule
```

* Works fine for small mux.
* But not scalable for larger muxes (e.g., 32:1).

---

### For Loop in Mux

```verilog
module mux_generate (input i0 , input i1, input i2 , input i3 , input [1:0] sel  , output reg y);
wire [3:0] i_int;
assign i_int = {i3,i2,i1,i0};
integer k;

always @ (*) begin
    for(k = 0; k < 4; k=k+1) begin
        if(k == sel)
            y = i_int[k];
    end
end
endmodule
```

Scalable design using `for` loop.

*Screenshot here: waveform showing correct mux output*

---

### Generate For Loop – Ripple Carry Adder

```verilog
module rca (input [7:0] num1 , input [7:0] num2 , output [8:0] sum);
wire [7:0] int_sum;
wire [7:0] int_co;

genvar i;
generate
    for (i = 1 ; i < 8; i=i+1) begin
        fa u_fa_1 (.a(num1[i]),.b(num2[i]),.c(int_co[i-1]),.co(int_co[i]),.sum(int_sum[i]));
    end
endgenerate

fa u_fa_0 (.a(num1[0]),.b(num2[0]),.c(1'b0),.co(int_co[0]),.sum(int_sum[0]));

assign sum[7:0] = int_sum;
assign sum[8]   = int_co[7];
endmodule
```

* `generate for` loop replicates **full adder hardware** automatically.
* Saves coding effort for large designs like adders, multipliers, shift registers.

*Screenshot here: schematic from Yosys*

---

## Key Takeaways

* Always add **default cases** in combinational logic.
* Avoid **incomplete if/case** constructs → prevents inferred latches.
* Ensure **no overlap** in case statements.
* Use **for loops** for scalable combinational logic.
* Use **generate-for loops** to instantiate hardware multiple times.

---

