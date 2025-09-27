

# README – If/Case Constructs and Looping Constructs in Verilog

##  Table of Contents

1. [Introduction](#introduction)
2. [If vs Case](#if-vs-case)
3. [Inferred Latches](#inferred-latches)
   * [Incomplete If Example](#incomplete-if-example)
   * [Incomplete Case Example](#incomplete-case-example)
   * [Partial Case Assignment](#partial-case-assignment)
   * [Overlapping Case Example](#overlapping-case-example)
4. [Looping Constructs](#looping-constructs)
   * [4:1 Mux using Case](#41-mux-using-case)
   * [For Loop in Mux](#for-loop-in-mux)
   * [Generate For Loop – Ripple Carry Adder](#generate-for-loop--ripple-carry-adder)
5. [Key Takeaways](#key-takeaways)

---

## Introduction

In Verilog, conditional constructs (`if` and `case`) and looping constructs (`for` and `generate for`) play an important role in writing efficient RTL code.
However, if they are not used properly, they may lead to **synthesis issues** such as **inferred latches** or **ambiguous hardware mappings**.

This document explains common pitfalls with examples, and also shows how loops can simplify repetitive hardware design.

---

## If vs Case

* `if` statements → used when **priority** is important (like priority encoders).
* `case` statements → used when **parallel checking** is required (like multiplexers).

Problem: Incomplete assignments in either construct can cause synthesis tools to infer **latches**, which are not allowed in purely combinational logic.

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

Since `y` is not assigned when `i0 = 0`, synthesis inserts a **latch**.

**Screenshot here**: (insert Yosys/GTKwave showing latch inference).

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

Missing `default` branch → latch inferred when `sel=2’b10` or `2’b11`.

**Screenshot here**: synthesis result with latch.

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
        2'b01 : y = i1;  //  x missing
        default : begin
                   x = i1;
                   y = i2;
              end
    endcase
end
endmodule
```

 In branch `2’b01`, `x` is not updated → latch inferred for `x`.

 **Screenshot here**: waveform mismatch.

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

 Here, `2’b10` and `2’b1?` overlap → synthesis tool may optimize differently, causing mismatches.

 **Screenshot here**: Yosys `show` output with overlapping logic.

---

## Looping Constructs

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

Easy for small mux, but **not scalable** for bigger muxes.

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

Uses a **for loop** to make the mux scalable.

**Screenshot here**: waveform showing correct output.

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

**Generate-for loop** is used to replicate hardware instances automatically.

**Screenshot here**: schematic from Yosys `show`.

---

## Key Takeaways

* Always use **default cases** in combinational logic.
* Avoid **incomplete if/case constructs** to prevent inferred latches.
* Ensure no **overlap in case statements**.
* Use **for loops** for scalable combinational logic.
* Use **generate-for loops** for instantiating repeated hardware.

---


