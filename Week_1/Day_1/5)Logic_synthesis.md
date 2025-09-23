

# Logic Synthesis

---

## RTL Design

* **RTL (Register Transfer Level)** design is the **behavioral representation** of the required specification.
* It describes how **data flows between registers** and how the **logic operates** on that data.
  <br>
![RTL design](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/1db990bb1253f71056cf377fae5743dc0f3a0b23/Week_1/Day_1/Pictures/RTL%20design.png)

---

## RTL to Digital Logic Circuit

* The **RTL code** (written in Verilog) is converted into a **digital logic circuit** during **logic synthesis**.
* This process maps the high-level design into **logic gates and flip-flops** available in the technology library.
* The **output** of synthesis is a **netlist**, which is a **gate-level representation** of the design.

 <br>
![synthesis process](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/1db990bb1253f71056cf377fae5743dc0f3a0b23/Week_1/Day_1/Pictures/Synthesis%20Process.png)
---

## `.lib` File (Library)

* The `.lib` file is the **standard cell library**, which provides details about the cells available in the technology.
* It contains **timing, area, and power characteristics** of each gate.
* Example cells:

  * 2-input AND gates (slow, medium, fast)
  * 3-input AND gates (slow, medium, fast)
  * 4-input AND gates (slow, medium, fast)
  * Flip-flops, inverters, multiplexers, etc.

![.lib](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/1db990bb1253f71056cf377fae5743dc0f3a0b23/Week_1/Day_1/Pictures/.lib.png)

---

## Why Different Flavors of Gates?

Different flavors exist because **delay, power, and area trade-offs** must be managed.

![Different flavors of gate](https://github.com/DHANASRI-A/RISC-V-Chip-Tapeout/blob/1db990bb1253f71056cf377fae5743dc0f3a0b23/Week_1/Day_1/Pictures/Different%20flavours%20of%20gate.png)

---

### 1. Fast Library (Low Delay Gates)

* Used when **high-speed operation** is required.
* **Reason:** The **combinational delay** in the logic path determines the **maximum operating frequency**.

#### Timing Condition

<h3 align="center">
  <b>T<sub>clk</sub> &gt; T<sub>cq_a</sub> + T<sub>combi</sub> + T<sub>setup_b</sub></b>
</h3>

* **T<sub>clk</sub>**: Clock period
* **T<sub>cq\_a</sub>**: Clock-to-Q delay of flip-flop A
* **T<sub>combi</sub>**: Delay of combinational logic
* **T<sub>setup\_b</sub>**: Setup time of flip-flop B

**Meaning:** The clock must be long enough to allow data to propagate from one flip-flop to the next.

#### Maximum Frequency

<h3 align="center">
  <b>f<sub>max</sub> = 1 / T<sub>clk(min)</sub></b>
</h3>

* **T<sub>clk(min)</sub>**: Minimum clock period that satisfies the above condition.
* Using **fast gates** → T<sub>combi</sub> decreases → T<sub>clk(min)</sub> decreases → f<sub>max</sub> increases.

**Fast gates → Higher performance (but higher power consumption).**

---

### 2. Slow Library (High Delay, Low Power Gates)

* Used when **power saving** is more important than speed.
* Example: In **battery-powered devices** where frequency is not critical.
* Since gates are slower → T<sub>combi</sub> increases → design speed decreases.
* But these cells consume **less dynamic and leakage power**.

**Slow gates → Lower speed but energy-efficient.**

---

## Summary

| Library Type | Feature          | Advantage                      | Trade-off                |
| ------------ | ---------------- | ------------------------------ | ------------------------ |
| **Fast**     | Low delay cells  | Higher frequency (performance) | Higher power consumption |
| **Slow**     | High delay cells | Low power consumption          | Lower speed              |

---

 **Conclusion:**
The link between **RTL code** and the **digital circuit** is established through **logic synthesis**, where the RTL is mapped onto different flavors of gates from the `.lib` file depending on the design’s **speed vs power requirements**.

---


