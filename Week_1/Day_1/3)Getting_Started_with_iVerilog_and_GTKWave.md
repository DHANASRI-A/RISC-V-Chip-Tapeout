## How to Work with iVerilog and GTKWave

We will use the example Verilog files provided in the cloned repository under the folder:

`verilog_files/`

Each example contains:

* **Design file** (e.g., `good_mux.v`) – contains the actual hardware logic.
* **Testbench file** (e.g., `tb_good_mux.v`) – provides input signals to test the design.

---

### **Step-by-Step Guide**

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
Run the following command to compile both the design and the testbench:

```bash
iverilog good_mux.v tb_good_mux.v
```

* **Output:** `a.out` file
* This file is the **compiled simulation program** that can be executed to simulate the design.

---

**Step 4: Run the simulation**
Execute the compiled simulation file:

```bash
./a.out
```

* **Output:** `tb_good_mux.vcd` file
* This is the **Value Change Dump (VCD)** file that contains all signal changes over time.

---

**Step 5: View the waveform using GTKWave**
Open the generated VCD file in GTKWave to visualize the signals:

```bash
gtkwave tb_good_mux.vcd
```

* **Output:** A waveform window showing all the signals in your design.
* You can see how **inputs change over time** and how the **outputs respond**, which helps verify the correctness of your design.

---

### **Summary of the Flow**

**Design + Testbench → iVerilog (compile) → a.out → Run Simulation → VCD File → GTKWave (view signals)**

---




