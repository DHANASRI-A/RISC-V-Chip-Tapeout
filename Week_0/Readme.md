
# VSD RISC-V Reference SoC Tapeout Program – Week 0

📅 **Duration**: Thu, 18 Sep 2025 – Wed, 24 Sep 2025
📌 **Theme**: Environment Setup + RTL Simulation Basics

---

## 📝 Week 0 Tasks

###  Tool Installation

Installed the following open-source EDA tools:

* **Yosys** – RTL synthesis
* **Icarus Verilog (iverilog)** – Simulation
* **GTKWave** – Waveform visualization

---

## 💻 System Requirements

| Resource | Requirement      |
| -------- | ---------------- |
| RAM      | 6 GB or higher   |
| HDD      | 50 GB free space |
| CPU      | 4 vCPUs          |
| OS       | Ubuntu 20.04+    |

Virtualization: [Oracle VirtualBox Download Link](https://www.virtualbox.org/wiki/Downloads)

---

## ⚙️ Installation Instructions

### **Yosys Installation**

```bash
$ sudo apt-get update
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys
$ sudo apt install make
$ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make config-gcc
$ git submodule update --init --recursive   # Required for abc submodule
$ make
$ sudo make install
```

Check installation:

```bash
yosys -V
```

---

### **Icarus Verilog (iverilog) Installation**

```bash
$ sudo apt-get update
$ sudo apt install iverilog
```

Check installation:

```bash
iverilog -V
```

---

### **GTKWave Installation**

```bash
$ sudo apt-get update
$ sudo apt install gtkwave
```

Run to confirm:

```bash
gtkwave
```

---

## 📊 Installation Summary

| Tool               | Purpose           | Version Check Command | Status      |
| ------------------ | ----------------- | --------------------- | ----------- |
| **Yosys**          | RTL Synthesis     | `yosys -V`            | ✅ Installed |
| **Icarus Verilog** | Simulation        | `iverilog -V`         | ✅ Installed |
| **GTKWave**        | Waveform Analysis | `gtkwave`             | ✅ Installed |

---

## 📸 Proof of Installation

![Tool Installation Screenshot](screenshot.png)

---

## ✅ Week 0 Deliverables

* GitHub repository created and initialized.
* Yosys, Icarus Verilog, and GTKWave successfully installed.
* Environment ready for RTL design and simulation from Week 1.

---


