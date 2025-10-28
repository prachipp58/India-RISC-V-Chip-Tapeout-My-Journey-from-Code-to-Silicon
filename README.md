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
---
**Output Visualization (Task 1):**
![Alt text](https://github.com/prachipp58/India-RISC-V-Chip-Tapeout-My-Journey-from-Code-to-Silicon/blob/main/images/week1/WhatsApp%20Image%202025-09-27%20at%2011.05.27%20PM.jpeg)

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
---

**Output Visualization (Task 1):**

![Alt text](https://github.com/prachipp58/India-RISC-V-Chip-Tapeout-My-Journey-from-Code-to-Silicon/blob/main/images/week1/WhatsApp%20Image%202025-09-27%20at%2011.23.59%20PM.jpeg)


### 🧪**Experiment 2: Flattened Synthesis**

**Goal:** Remove all internal hierarchy from the netlist, resulting in a single module containing all logic gates.

| # | Command Executed | Description | Key Learnings/Notes |
| :---: | :--- | :--- | :--- |
| **1** | `flatten` | **Removes all module hierarchy.** | Converts the design into a single, flat netlist structure. |
| **2** | `show` | **Visualizes the Flattened Netlist.** | **Output:** Diagram shows all gate-level cells merged into one large, complex block. |
| **3** | `write_verilog -noattr multiple_modules_flat.v` | **Saves the Flattened Gate-Level Netlist.** | Ready for tools that require a single netlist block. |
| **4** | `exit` | **Exits the Yosys shell.** | Returns control to the Linux shell. (Assuming you re-enter for Task 3). |
---
**Output Visualization (Task 2):**

![Alt text](https://github.com/prachipp58/India-RISC-V-Chip-Tapeout-My-Journey-from-Code-to-Silicon/blob/main/images/week1/WhatsApp%20Image%202025-09-27%20at%2011.30.04%20PM.jpeg)

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

![Alt text](https://github.com/prachipp58/India-RISC-V-Chip-Tapeout-My-Journey-from-Code-to-Silicon/blob/main/images/week1/WhatsApp%20Image%202025-09-27%20at%2011.30.20%20PM.jpeg)

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
![Alt text](https://github.com/prachipp58/India-RISC-V-Chip-Tapeout-My-Journey-from-Code-to-Silicon/blob/main/images/week1/WhatsApp%20Image%202025-09-27%20at%2011.37.05%20PM.jpeg)

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
![Alt text](https://github.com/prachipp58/India-RISC-V-Chip-Tapeout-My-Journey-from-Code-to-Silicon/blob/main/images/week1/WhatsApp%20Image%202025-09-27%20at%2011.37.54%20PM.jpeg)

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
![Alt text](https://github.com/prachipp58/India-RISC-V-Chip-Tapeout-My-Journey-from-Code-to-Silicon/blob/main/images/week1/WhatsApp%20Image%202025-10-01%20at%2011.04.01%20AM.jpeg)

## 💻 Day 4: Comprehensive Analysis of Synthesis-Simulation Mismatch

Day 4 focused on **Gate-Level Simulation (GLS)**, a crucial verification step after synthesis, with the primary goal of demonstrating and debugging the **Synthesis-Simulation Mismatch** caused by incorrect RTL coding practices.

***

## 1. 🧪 Experiment: Synthesis-Simulation Mismatch and Netlist Validation

This experiment demonstrates a synthesis-simulation mismatch caused by using an **incorrect or incomplete sensitivity list** in the combinatorial `always` block of the design file, `bad_mux.v`. This type of error causes the Verilog simulator to behave differently from the physical netlist.

### Step 1: Pre-Synthesis (RTL) Simulation (RTL Pass - Misleading)

The initial simulation of the original RTL code (`bad_mux.v`) runs successfully, masking the underlying bug.

| Command Sequence | Observation |
| :--- | :--- |
| ```bash iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux.v tb_bad_mux.v ./a.out ``` | **RTL PASS:** The simulator follows its internal scheduling rules, honoring the explicit (but incorrect) sensitivity list. The output appears **functionally correct**, misleading the designer. |
---
![Alt text](https://github.com/prachipp58/India-RISC-V-Chip-Tapeout-My-Journey-from-Code-to-Silicon/blob/main/images/week1/WhatsApp%20Image%202025-10-01%20at%2011.34.55%20AM.jpeg)

### Step 2: Synthesis and Netlist Analysis

Yosys synthesizes the flawed RTL. Instead of inferring a latch (the classic error), the tool correctly infers purely combinatorial logic (a MUX) but flags the incorrect sensitivity list as a warning.

| Yosys Log Highlight | Implication |
| :--- | :--- |
| **Note:** `Recommending use of @* instead of @(...)` | Confirms the use of an incomplete sensitivity list, which is the **RTL bug**. |
| **Check:** `No latch inferred for signal \bad_mux.\y'` | Confirms the netlist contains **purely combinatorial logic** (`sky130_fd_sc_hd__mux2_1`), meaning the hardware updates instantly with any input change. |
---
![Alt text](https://github.com/prachipp58/India-RISC-V-Chip-Tapeout-My-Journey-from-Code-to-Silicon/blob/main/images/week1/WhatsApp%20Image%202025-10-01%20at%2011.35.27%20AM.jpeg)
### Step 3: Post-Synthesis (GLS) Simulation (GLS Fail - Mismatch Exposed)

The Gate-Level Simulation (GLS) using the synthesized netlist (`synth_bad_mux.v`) exposes the functional mismatch between the ideal RTL simulation and the actual netlist behavior.

| Command Sequence | Result |
| :--- | :--- |
| ```bash # Run GLS on the synthesized netlist iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v synth_bad_mux.v tb_bad_mux.v ./a.out ``` | **GLS FAIL (Mismatch):** The **Netlist (GLS)** output updates **instantly** when any input changes (correct hardware behavior). The overlaid **RTL waveform** shows a **functional failure** or **delayed update** where the output failed to update instantly due to the missing signal in the sensitivity list. |
---
![Alt text](https://github.com/prachipp58/India-RISC-V-Chip-Tapeout-My-Journey-from-Code-to-Silicon/blob/main/images/week1/WhatsApp%20Image%202025-10-01%20at%2011.35.55%20AM.jpeg)

### 🛑 Final Conclusion and Verification Status

The experiment successfully demonstrated the critical nature of the **Synthesis-Simulation Mismatch**. The initial **RTL Simulation passed (✅)**, but the subsequent **Gate-Level Simulation (GLS) failed (❌)** to match the RTL, proving that the **missing signal in the sensitivity list** led to functionally incorrect hardware behavior compared to the flawed simulator model. The correct fix is to use **`always @*`** in the RTL.
## 🧪 Experiment 2: Synthesis-Simulation Mismatch (Blocking Assignments in Sequential Logic)

This experiment demonstrates a major functional error caused by incorrectly using **blocking assignments (`=`)** within a sequential `always @(posedge clk)` block in the file **`blocking_caveat.v`**. This causes a mismatch because the simulator executes the assignments sequentially, while the synthesizer infers parallel D-Flip-Flops (DFFs), losing the intended sequential flow (e.g., a shift register).

### Step 1: Pre-Synthesis (RTL) Simulation (RTL Pass - Misleading)

The RTL code, using blocking assignments (`=`), passes the simulation check, masking the bug.

| Command Sequence | Observation |
| :--- | :--- |
| ```bash iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat.v tb_blocking_caveat.v ./a.out VCD info: dumpfile tb_blocking_caveat.vcd opened for output. tb_blocking_caveat.v:24: $finish called at 3000000 (1ps) ``` | **RTL PASS:** Simulator executes assignments **sequentially** (correcting the designer's intent) and the logic appears functional (e.g., shifting) in simulation. |
---
![Alt text](https://github.com/prachipp58/India-RISC-V-Chip-Tapeout-My-Journey-from-Code-to-Silicon/blob/main/images/week1/WhatsApp%20Image%202025-10-01%20at%2011.11.07%20AM.jpeg)

### Step 2: Synthesis and Netlist Analysis

The synthesizer (Yosys) correctly infers **parallel D-Flip-Flops (DFFs)** based on the clock edge, **ignoring the sequential effect** of the blocking assignment operator (`=`).

| Synthesis Step | Implication |
| :--- | :--- |
| **Yosys Flow:** `synth -top blocking_caveat` | Yosys will infer **parallel DFFs** (using `$dff` cells) because it correctly ignores the sequential semantics of the blocking operator in a clocked block. |
| **Netlist Structure:** Parallel DFFs | The netlist will feature **parallel DFFs**, destroying the intended sequential dependency (e.g., the shifting action). |
---
![Alt text](https://github.com/prachipp58/India-RISC-V-Chip-Tapeout-My-Journey-from-Code-to-Silicon/blob/main/images/week1/WhatsApp%20Image%202025-10-01%20at%2011.23.30%20AM%20(1).jpeg)

### Step 3: Post-Synthesis (GLS) Simulation (GLS Fail - Mismatch Exposed)

The GLS on the synthesized netlist (`synth_blocking_caveat.v`) will expose the functional failure, demonstrating the mismatch between the intended sequential flow and the synthesized parallel hardware.

| Command Sequence | Result |
| :--- | :--- |
| ```bash # Run GLS on the synthesized netlist iverilog ... sky130_fd_sc_hd.v synth_blocking_caveat.v tb_blocking_caveat.v ./a.out ``` | **GLS FAIL (Mismatch):** The **Netlist (GLS)** output exhibits **parallel behavior** (all DFFs updating from the same source) instead of the intended sequential/shift register behavior. |
---
![Alt text](https://github.com/prachipp58/India-RISC-V-Chip-Tapeout-My-Journey-from-Code-to-Silicon/blob/main/images/week1/WhatsApp%20Image%202025-10-01%20at%2011.12.56%20AM.jpeg)

### 🛑 Final Conclusion for Experiment 2

The experiment successfully demonstrated the functional failure caused by the **synthesis-simulation mismatch**. The intended sequential behavior was corrupted during synthesis because the **blocking assignment operator (`=`)** was used in a clocked block.

| Status | Result | Fix |
| :--- | :--- | :--- |
| **Mismatch Status** | **GLS Failed (❌)** | Replace all blocking assignments (`=`) with **non-blocking assignments (`<=`)** in sequential (`always @(posedge clk)`) blocks. |

## 💻 Day 5: RTL Optimization and Synthesis Best Practices

Day 5 focused on advanced RTL coding practices essential for efficient synthesis, primarily analyzing how complex structures like conditional statements (`if-else`, `case`) and repetition constructs (`generate`) map to efficient hardware. The central objective was to understand the synthesis tool's interpretation and **eliminate unintentional latch inference** and **avoid area/timing bottlenecks**.

## 1. 🛑 Synthesis Caveat 1: Avoiding Unintentional Latch Inference

The first set of labs highlighted the critical error of writing **incomplete combinatorial logic**, which forces the synthesizer (Yosys) to infer latches. Latches are generally undesirable in synchronous design because they complicate timing analysis.

### Observation: Incomplete Conditional Logic

| RTL Construct | Synthesis Implication | Verification Status |
| :--- | :--- | :--- |
| **Incomplete `if-else`** (Missing final `else`) | The output variable is not assigned under all possible conditions. Yosys is forced to infer a **Transparent Latch** to hold the previous value. | **GLS Failure (❌)** |
| **Incomplete `case`** (Missing `default` case) | The output state is ambiguous when the selector doesn't match any defined case. This also results in a **Transparent Latch** inference. | **GLS Failure (❌)** |

### Best Practice 💡
Always use the **`always @*`** syntax and ensure every output signal is assigned a value in **all branches** of the conditional logic (`if/else` or `case/default`) within a combinatorial block.

## 2. ⚡ Synthesis Caveat 2: Structure vs. Performance (`if` vs. `case`)

This experiment analyzed the hardware structures resulting from priority-based versus parallel conditional logic, showing how RTL choice directly impacts the resulting critical path.

### Observation: Priority Encoding vs. Parallelism

| RTL Construct | Hardware Mapping | Performance Implication |
| :--- | :--- | :--- |
| **Cascading `if-else if`** | Maps directly to a **Priority Encoder Chain**. The first condition has the highest priority. | **Slower (Critical Path)** increases linearly with the number of conditions, as each stage must wait for the previous one to fail. |
| **`case` Statement** | Maps to a highly parallel **Multiplexer (MUX) Tree** structure. | **Faster (Area/Speed Trade-off)**. Provides near-simultaneous evaluation, resulting in a shallower, faster logic path. |

### Best Practice 💡
Use **`case` statements** (and ensure they are complete) for decoding parallel/mutually exclusive conditions, and restrict cascading **`if-else if`** logic only where genuine priority is required.

## 3. ⚙️ Synthesis Best Practice: Scalability with `generate`

The final lab series focused on generating highly repetitive and scalable hardware structures using **`for generate`** blocks, a crucial technique for large-scale physical design.

### Observation: Compile-Time Instantiation

| RTL Construct | Execution Time | Synthesizable Use Case |
| :--- | :--- | :--- |
| **Standard `for` loop** | Runtime (Simulation only) | Used for control/sequencing in a testbench or sequential block. **Not synthesizable for hardware replication.** |
| **`for generate`** | **Compile-time** (Synthesis only) | Essential for creating **parallel arrays** of hardware (e.g., registers, adders, I/O buffers) by instantiating modules based on a loop count. |

### Best Practice 💡
For creating scalable, repetitive parallel hardware—such as the array of DFFs demonstrated in the lab—the **`for generate`** loop with a **`genvar`** must be used. This allows the synthesizer to efficiently replicate the target standard cells across the netlist.

# 🎓 Week 1 Key Learnings: From RTL to Synthesis Optimization

This week covered the fundamental digital VLSI flow, emphasizing practical RTL coding for efficient hardware synthesis using the **Sky130 PDK** and **Yosys**.

## 1. ⚙️ Core Synthesis & Mapping
* **Translation:** Successfully converted abstract RTL into technology-specific gate-level netlists using **Yosys** and the **ABC mapper**.
* **Foundation:** Confirmed functional equivalence of basic combinatorial and sequential designs (MUX, D-FFs) after mapping to **Sky130 standard cells**.

## 2. ❌ Synthesis-Simulation Mismatch (The Debug Focus)
* **Sensitivity Error:** Demonstrated that incomplete combinatorial logic (`always @(...)` missing signals) leads to **GLS failure (❌)**, requiring the use of **`always @*`** or complete assignment coverage.
* **Sequential Error:** Proved that using **blocking assignments (`=`)** in clocked (`always @(posedge clk)`) blocks destroys intended sequential logic (like shift registers), confirming the mandate for **non-blocking assignments (`<=`)**.

## 3. 💡 RTL Optimization & Best Practices
* **Latch Avoidance:** Learned that failing to assign an output in all conditional branches (`if/else` or `case/default`) forces the synthesizer to infer an unwanted **Latch**.
* **Performance:** Understood that deep **`if-else if`** chains create slow **Priority Encoder** logic, favoring parallel **`case` statements** for speed.
* **Scalability:** Mastered the **`for generate`** construct, essential for creating scalable, parallel hardware arrays (e.g., arrays of DFFs) at compile-time.
</details>
<details>
<summary><b> 📅 Week 2- System-on-Chip (SoC) Fundamentals and VSDBabySoC Functional Verification </b></summary>
---

## 🌟 Project Goal
To develop a **solid understanding** of **System-on-Chip (SoC)** design fundamentals and apply this knowledge by practicing **functional modelling** of the **BabySoC** using **Icarus Verilog** and **GTKWave**.

---

## Part I: Conceptual Foundations of SoC Design

### I. What is a System-on-Chip (SoC)?

A **System-on-Chip (SoC)** is an **integrated circuit (IC)** that integrates *all* or most components of a computer or other electronic system into a **single silicon chip**. This architecture provides critical benefits for modern electronics, such as smaller size, lower power consumption, and improved performance due to component proximity.

#### Anatomy of a Typical SoC 🧠

A typical SoC is a complex architecture made up of four primary, interconnected blocks:

1.  **CPU (Central Processing Unit) / Processor Core:** The computational "brain" responsible for executing software instructions and controlling the system.
2.  **Memory Subsystem:** Includes different types of memory (like SRAM and DRAM) and the necessary controllers for storing code and data for the CPU.
3.  **Peripherals:** These are specialized Input/Output (I/O) blocks that enable the SoC to interact with the outside world and perform specific functions (e.g., UARTs, Timers, ADCs).
4.  **Interconnect:** The communication highway (Bus or Network-on-Chip) that allows all the components (CPU, memory, peripherals) to efficiently transfer data.

---

### II. Why BabySoC is Your Learning Supertool 👶💻

The **BabySoC** is a **simplified model** designed specifically for learning core SoC concepts.

A full commercial SoC is overwhelming due to the sheer complexity of integrating dozens of heterogeneous components and advanced interconnect standards (like AMBA AXI).

**BabySoC strips away this complexity**, offering a focused, bite-sized environment:

* It contains the **core architectural elements** (CPU, Memory, simplified Peripherals, and an Interconnect) in a minimal configuration.
* It allows us to **clearly observe the data flow** and interaction between components, making fundamental concepts tangible.
* It provides a **safe, contained environment** to practice essential skills like functional modelling and verification before tackling industrial-scale systems.

In essence, BabySoC acts as the **ideal training platform** to build foundational knowledge in SoC development.

---

### III. The Critical Role of Functional Modelling 📝

**Functional modelling** is an essential step that occurs **before** the **RTL (Register-Transfer Level) design** and **physical design** stages.

Its primary goal is to **verify the system's intended behavior** and **correctness** at a high level of abstraction, acting as the **"measure twice, cut once"** phase of chip design.

| Design Stage | Focus | Key Role (Why it's essential) |
| :--- | :--- | :--- |
| **1. Functional Modelling** | *What* the system should do. High-level algorithm and system architecture. | **Fast verification** of the design specification and system architecture *before* committing to hardware structure. |
| **2. RTL Design** | *How* the system implements the function using registers and logic gates (Verilog/VHDL code). | Specifies the hardware logic for synthesis. |
| **3. Physical Design** | *Where* the transistors are placed on the silicon chip (Layout, timing closure). | Creates the final manufacturable silicon mask. |

By using tools like **Icarus Verilog** for simulation and **GTKWave** for waveform visualization, we ensure that the BabySoC's logic is **functionally correct** *before* wasting significant time and resources on developing the full RTL description or moving to physical layout.

---
## Part II: Hands-on Functional Verification

### 2.1 Toolchain Setup and TLV Conversion

The RVMYTH core is written in **TL-Verilog (.tlv)**, which requires conversion to standard Verilog before simulation.

| Step | Command Executed | Purpose |
| :--- | :--- | :--- |
| **1. TLV Conversion** | `sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --outdir ./src/module/` | Translates the high-level RVMYTH source into standard RTL. |
| **2. Compilation** | `iverilog -o output/pre_synth_sim/pre_synth_sim.out -DPRE_SYNTH_SIM ...` | Compiles the Testbench, RVMYTH, PLL, and DAC modules into an executable binary. |
| **3. Simulation** | `./pre_synth_sim.out` | Executes the binary, running the CPU's program and generating the `pre_synth_sim.vcd` waveform file. |

#### Simulation Logs

```bash
# Log for TLV -> Verilog Conversion (sandpiper-saas)
[INSERT TLV CONVERSION LOG HERE]

# Log for Icarus Verilog Compilation (iverilog)
[INSERT IVERILOG COMPILATION LOG HERE]
```
</details>
<details>
<summary><b> 📅 Week 3- Post Synthesis GLS & STA Fundamentals </b></summary>

</details>
<details>
<summary><b> 📅 Week 4- CMOS Circuit Design & Analysis using SkyWater 130nm PDK </b></summary>

</details>
<details>
<summary><b> 📅 Week 5- OpenROAD Flow Setup and Floorplan + Placement </b></summary>

</details>

