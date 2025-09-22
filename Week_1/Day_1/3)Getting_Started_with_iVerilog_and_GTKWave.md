

## How to Work with iVerilog and GTKWave

To get familiar with simulation tools, we will use the **example Verilog files** provided in the cloned repository under the folder:

```
verilog_files/
```

Each example contains:

* **Design file** (e.g., `good_mux.v`) – contains the actual hardware logic.
* **Testbench file** (e.g., `tb_good_mux.v`) – provides input signals to test the design.

---

### Step-by-Step Guide

**Step 1: Navigate to the folder**
Open your terminal and go to the folder containing the Verilog files:

```bash
cd VSD/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```

> This ensures all commands and outputs are executed in the correct directory.

---

**Step 2: Choose a design and testbench**

* Pick any Verilog design file from the folder.
* Each design file has a corresponding testbench.

> Example: `good_mux.v` (design) and `tb_good_mux.v` (testbench)

---

**Step 3: Compile the files using iVerilog**

```bash
iverilog good_mux.v tb_good_mux.v
```

* **Output:** `a.out` file
* This file is the **compiled simulation program** that can be executed to simulate the design.

---

**Step 4: Run the simulation**

```bash
./a.out
```

* **Output:** `tb_good_mux.vcd` file
* This is the **Value Change Dump (VCD)** file that records all signal changes over time.

---

**Step 5: View the waveform using GTKWave**

```bash
gtkwave tb_good_mux.vcd
```

* **Output:** A waveform window showing all signals in your design.
* You can observe how **inputs change over time** and how the **outputs respond**, which helps verify the correctness of your design.

---

### Summary of the Flow

```
Design + Testbench
        │
        ▼
     iVerilog
        │
        ▼
      a.out
        │
        ▼
     VCD File
        │
        ▼
GTKWave (View Signals)
```

---

### Step Summary Table

| Step | Command                             | Output            | Description                                     |
| ---- | ----------------------------------- | ----------------- | ----------------------------------------------- |
| 1    | `cd verilog_files`                  | –                 | Navigate to the folder containing Verilog files |
| 2    | Choose design + testbench           | –                 | Pick a design and its corresponding testbench   |
| 3    | `iverilog good_mux.v tb_good_mux.v` | `a.out`           | Compile the design and testbench                |
| 4    | `./a.out`                           | `tb_good_mux.vcd` | Run the simulation to generate the VCD file     |
| 5    | `gtkwave tb_good_mux.vcd`           | GTKWave window    | View the waveform signals                       |

---




