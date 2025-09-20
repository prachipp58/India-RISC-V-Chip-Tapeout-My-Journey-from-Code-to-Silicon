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
#### **2. Iverilog**
```bash
$ sudo apt-get update
$ sudo apt-get install iverilog
```


#### **3.gtkwave**
```bash
$ sudo apt-get update
$ sudo apt install gtkwave
```
