# India RISC-V Chip Tapeout: A Journey from Code to Silicon
My participation in the India RISC-V Chip Tapeout Program is a commitment to a larger mission: to deeply understand the principles of VLSI design and contribute to India's "Silicon to Sovereignty" initiative. This repository serves as a personal portfolio, showcasing my learning, challenges, and successes on this journey.
<details>
<summary><b> 📅 Week 0 - Tools Installation</b></summary>
  
## Week 0: The Foundry - Building the Foundation

### **Objective**
The objective of this week was to establish a fully functional open-source EDA (Electronic Design Automation) environment. This foundational step is critical, as a well-configured environment is the bedrock for all subsequent design and implementation. The machine configuration used for this task was **6GB RAM**, **50GB HDD**, **Ubuntu 20.04**, and **4vCPU**.

### 🛠️**Oracle virtual machine link**
https://www.virtualbox.org/wiki/Downloads 

✅ **Oracle virtual machine Successfully Installed**

### 💻**System Requirements**
- 6 GB RAM
- 50 GB HDD
- Ubuntu 20.04 or higher
- 4 vCPU

### ⚙️**Tool check**
#### **1. Yosys – RTL Synthesis Tool**
```bash
$ sudo apt-get update
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys
$ sudo apt install make               # If make is not installed
$ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make config-gcc
# Yosys build depends on a Git submodule called abc, which hasn't been initialized yet. You need to run the following command before running make
$ git submodule update --init --recursive
$ make 
$ sudo make install
```
## 📷 **Installation Verification**
<p align="center">
  <img src="https://github.com/prachipp58/India-RISC-V-Chip-Tapeout-My-Journey-from-Code-to-Silicon/blob/main/images/WhatsApp%20Image%202025-09-23%20at%201.39.01%20AM.jpeg" 
       alt="Yosys Installed" width="600"/>
</p>

<div align="center">

✅ **Yosys Successfully Installed**

</div>

---

#### **2. Iverilog**
```bash
$ sudo apt-get update
$ sudo apt-get install iverilog
```
## 📷 **Installation Verification**
<p align="center">
  <img src="https://github.com/prachipp58/India-RISC-V-Chip-Tapeout-My-Journey-from-Code-to-Silicon/blob/main/images/WhatsApp%20Image%202025-09-23%20at%201.39.30%20AM.jpeg" 
       alt="Iverilog Installed" width="600"/>
</p>

<div align="center">

✅ **Iverilog Successfully Installed**

</div>

---

#### **3.gtkwave**
```bash
$ sudo apt-get update
$ sudo apt install gtkwave
```
## 📷 **Installation Verification**
<p align="center">
  <img src="https://github.com/prachipp58/India-RISC-V-Chip-Tapeout-My-Journey-from-Code-to-Silicon/blob/main/images/WhatsApp%20Image%202025-09-23%20at%201.39.43%20AM.jpeg" 
       alt="GTKWave Installed" width="600"/>
</p>

<div align="center">

✅ **GTKWave Successfully Installed**

</div>

---
</details>
<details>
<summary><b> 📅 Week 1 - Verilog RTL Design, Synthesis Fundamentals, and Optimization</b></summary>
  
### Day 1 - Introduction to Verilog RTL design and Synthesis

### 🛠️ Focus: RTL Synthesis of `good_mux.v` using Yosys & ABC

This log documents the commands for environment setup and the initial synthesis flow, highlighting key observations and discrepancies.

| # | Command Executed | Description | Lab Context/Tool | Key Learnings/Notes |
| :---: | :--- | :--- | :--- | :--- |
| **1** | `git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop` | **Clones the official workshop repository.** | ⚙️ Git / Setup | Creates the local directory containing all lab files. |
| **2** | `cd sky130RTLDesignAndSynthesisWorkshop` | **Changes to the working directory.** | 📁 Linux Shell | Sets the base path for running EDA tools. |
| **3** | `yosys` | **Launches the Yosys synthesis tool** command-line interface. | ▶️ Yosys | Starts the prompt (`yosys>`). |
| **4** | `read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib` | **Loads the sky130 standard cell library.** | 📚 Yosys | Imported **418** cells. ⚠️ **CRITICAL NOTE:** Reference showed 428; must verify library version. |
| **5** | `read_verilog good_mux.v` | **Loads the RTL Design Under Test (DUT).** | 📜 Yosys | Successfully parses the Verilog design file. |
| **6** | `synth -top good_mux` | **Executes the core synthesis script** (cleanup, flattening, optimization). | ⚙️ Yosys | Prepares the logic for technology mapping. |
| **7** | `abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib` | **Performs final logic optimization and technology mapping.** | 🚀 Yosys (via ABC) | Mapped optimally to **1 `sky130_fd_sc_hd__mux2_1` cell** (✅ Superior/Optimal Mapping). |
| **8** | `show` | **Generates a graphical visualization** of the synthesized netlist. | 🖼️ Yosys (via Graphviz) | Visually confirms the netlist structure. |
| **9** | `!gvim good_mux_netlist.v` | **Spawns a shell command to inspect the gate-level netlist file.** | 🔍 Yosys / Shell | Confirms the gate-level Verilog instantiates the MUX cell. |
| **10** | `write_verilog -noattr good_mux_netlist.v` | **Saves the final synthesized netlist to a file.** | 📝 Yosys | Creates the netlist file for subsequent Gate-Level Simulation (GLS). |
| **11** | `stat` | **Displays the final cell count and design hierarchy statistics.** | 📊 Yosys | Confirms final area/cell usage (1 MUX cell). |
| **12** | `exit` | **Exits the Yosys shell.** | 🚪 Yosys | Returns control to the Linux shell. |
</details>

