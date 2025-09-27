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
  
## 💻Day 1 - Introduction to Verilog RTL design and Synthesis

### 🎯 Focus: RTL Synthesis of `good_mux.v` using Yosys & ABC

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

## 💻Day 2 Log - Hierarchical Synthesis Experiments 🧠

### 🎯 Focus: Synthesis Modes (Hierarchical, Flat, and Block-Level)

This log documents three separate synthesis runs on the `multiple_modules.v` design to analyze the effects of hierarchy and targeted synthesis, providing outputs for visual documentation.

---

### 🧪**Experriment 1: Hierarchical Synthesis (Hierarchy Preserved)**

**Goal:** Synthesize the design while maintaining the structure of `sub_module1` and `sub_module2` as instantiations in the netlist.

| # | Command Executed | Description | Key Learnings/Notes |
| :---: | :--- | :--- | :--- |
| **1** | `read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib` | **Loads the sky130 standard cell library.** | Imported 418 cells (discrepancy noted). |
| **2** | `read_verilog multiple_modules.v` | **Loads the Hierarchical RTL Design.** | Parses all modules defined in the file. |
| **3** | `synth -top multiple_modules` | **Synthesizes the Top Module (No Flatten).** | Optimization runs while preserving module boundaries. |
| **4** | `abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib` | **Performs Technology Mapping on the hierarchical netlist.** | Maps standard cells but preserves hierarchy. |
| **5** | `show` | **Generates a graphical visualization.** | **Output:** Diagram shows the top module instantiating the sub-module boxes. |
| **6** | `write_verilog -noattr multiple_modules_hier.v` | **Saves the Hierarchical Gate-Level Netlist.** | File ready for hierarchical Gate-Level Simulation (GLS). |
| **7** | `!gvim multiple_modules_hier.v` | **Inspects the saved netlist file.** | Confirms instantiation of sub-modules. |

**Output Visualization (Task 1):**

[PLACEHOLDER: Image for Task 1 - Hierarchical Synthesis]

---

### 🧪**Experiment 2: Flattened Synthesis**

**Goal:** Remove all internal hierarchy from the netlist, resulting in a single module containing all logic gates.

| # | Command Executed | Description | Key Learnings/Notes |
| :---: | :--- | :--- | :--- |
| **1** | `flatten` | **Removes all module hierarchy.** | Converts the design into a single, flat netlist structure. |
| **2** | `show` | **Visualizes the Flattened Netlist.** | **Output:** Diagram shows all gate-level cells merged into one large, complex block. |
| **3** | `write_verilog -noattr multiple_modules_flat.v` | **Saves the Flattened Gate-Level Netlist.** | Ready for tools that require a single netlist block. |
| **4** | `exit` | **Exits the Yosys shell.** | Returns control to the Linux shell. (Assuming you re-enter for Task 3). |

**Output Visualization (Task 2):**

[PLACEHOLDER: Image for Task 2 - Flattened Synthesis]

---

### 🧪**Experiment 3: Sub-Module Level Synthesis**

**Goal:** Synthesize and analyze a single block (`sub_module1`) in isolation (essential for block-level closure).

| # | Command Executed | Description | Key Learnings/Notes |
| :---: | :--- | :--- | :--- |
| **1** | `yosys` | **Launches a new Yosys session.** | Required after the previous `exit`. |
| **2** | `read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib` | **Loads the library.** | Sets up the technology reference. |
| **3** | `read_verilog multiple_modules.v` | **Loads the RTL.** | Loads all modules for potential targeting. |
| **4** | `synth -top sub_module1` | **Synthesizes *only* the `sub_module1` design.** | **Key Experiment:** Isolates the synthesis target. |
| **5** | `abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib` | **Maps the isolated sub-module to cells.** | Generates the final netlist for this block. |
| **6** | `show` | **Visualizes the `sub_module1` netlist.** | **Output:** Diagram shows only the gate-level implementation of `sub_module1`. |
| **7** | `write_verilog -noattr sub_module1_netlist.v` | **Saves the netlist for the sub-module.** | Allows saving the block for future reuse as a hard macro. |
| **8** | `exit` | **Exits the Yosys shell.** | Concludes the block-level synthesis experiment. |

**Output Visualization (Task 3):**

[PLACEHOLDER: Image for Task 3 - Sub-Module Synthesis]

## 💻 Day 3: Synthesis, Optimization, and Visualization

### 🧪**Experiment 1: Synthesis and Optimization of `opt_check2.v`**

This experiment demonstrates the standard, optimized synthesis flow for a single-module design, preparing the Register-Transfer Level (RTL) code for technology mapping to the Skywater 130nm standard cell library.

#### Yosys Command Sequence

```bash
# 1. Load Technology Library (Sky130 PDK)
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# 2. Load the RTL Design File
yosys> read_verilog opt_check2.v

# 3. Initial Synthesis and Mapping
yosys> synth -top opt_check2

# 4. Remove unused elements and perform simple logic optimizations
yosys> opt_clean -purge

# 5. Advanced Technology Mapping and Logic Minimization using ABC
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# 6. Generate Final Statistics (Gate count, Area)
yosys> stat

# 7. Output the Final Gate-Level Netlist
yosys> write_verilog -noattr synth_opt_check2.v

# 8. Visualize the Synthesized Netlist
yosys> show
   ```

### 🧪**Experiment2: Hierarchical Synthesis of `multiple_module_opt2`**

This experiment successfully synthesized a hierarchical design, demonstrating the crucial role of the **`flatten`** pass in enabling global optimization and resource sharing across multiple module instances.

### Complete Synthesis Flow for Hierarchical Design

The following commands were executed in sequence to synthesize, flatten, and map the design.

```bash
# 1. Load Technology Library 
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# 2. Load Multi-Module Design (Includes sub_module and top module)
yosys> read_verilog multiple_module_opt2.v

# 3. Initial Synthesis and Hierarchy Management
yosys> synth -top multiple_module_opt2

# 4. CRUCIAL STEP: Flatten Hierarchy for Global Optimization
yosys> flatten

# 5. Clean up unused elements after flattening
yosys> opt_clean -purge

# 6. Technology Mapping and Logic Minimization using ABC
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# 7. Visualize the final optimized netlist
yosys> show
# This command generates the netlist visualization using an external viewer (e.g., xdot).
```
## 🧪 Experiment 3: Sequential Logic Mapping and Optimization

This experiment focuses on synthesizing a design containing sequential elements (D-Flip-Flops) and ensuring they are correctly mapped to the target Sky130 standard cells **before** the final combinatorial logic mapping (`abc`).

### Yosys Command Sequence

```bash
# 1. Load Technology Library (Sky130 PDK)
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# 2. Load the Sequential RTL Design File
yosys> read_verilog dff_const5.v

# 3. Initial Synthesis and RTL-to-Netlist Conversion
yosys> synth -top dff_const5

# 4. **CRUCIAL STEP: DFF Mapping**
# Replaces generic $dff cells with specific Sky130 Flip-Flop cells.
yosys> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# 5. Visualize the design after DFF mapping (optional, but instructive)
yosys> show 

# 6. Map Remaining Combinatorial Logic to Sky130 Gates
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# 7. Get final gate count and area statistics
yosys> stat

# 8. Output the Final Gate-Level Netlist
yosys> write_verilog -noattr synth_dff_const5.v
```
</details>

