# Floorplan and Placement of GCD 

This laboratory guide provides comprehensive instructions for implementing **Floorplanning** and **Placement** stages of the **Greatest Common Divisor (GCD)** circuit using **OpenROAD** Flow Scripts. The workflow demonstrates the critical transition from logical design representation to physical layout implementation in the VLSI backend design flow.

---

Table of Contents

1. [Overview]()
3. [Importance of Floorplanning and Placement](#importance-of-floorplanning-and-placement)

---

## Overview

This tutorial demonstrates the physical design implementation of a GCD circuit, focusing on two critical stages of the VLSI backend flow:

- **Floorplanning**: Defining the chip's physical boundaries, I/O pad placement, and macro positioning
- **Placement**: Positioning standard cells within the defined floorplan area

These stages establish the foundation for subsequent routing and timing closure operations.



## Prerequisites and Environment Setup

### System Requirements

Ensure your development environment meets the following specifications:

- **Operating System**: Ubuntu 20.04 or later (Ubuntu 22.04 recommended)

- **Version Control**: Git (latest stable version)
- **Build Toolchain**:

   - CMake (version 3.14 or higher)
   - GCC/G++ compiler suite
   - GNU Make
   - Standard development libraries

- **Python Environment**: Python 3.6 or higher

- **Required EDA Tools**
The following Electronic Design Automation tools must be available:

- **OpenROAD**: Physical design automation platform
- **Yosys**: Logic synthesis framework
- **OpenSTA**: Static timing analysis engine

---

## Importance of Floorplanning and Placement

Floorplanning is a crucial stage in the physical design process that defines the overall structure and layout of the chip prior to cell placement. It includes:

- Determining the die and core dimensions.
- Positioning I/O pads along the die periphery.
- Strategically arranging macros (such as SRAMs and PLLs).
- Organizing the core area into standard cell rows.
- Designing an efficient power distribution network (PDN).
- Reserving regions for physical-only cells (e.g., tap cells, endcaps, and blockages).

Placement follows floorplanning and involves arranging standard cells and smaller functional blocks within the defined core area. The main objectives are to:

- Minimize interconnect wire length between connected cells.
- Reduce routing congestion to facilitate efficient interconnections.
- Satisfy timing requirements to ensure reliable circuit performance.

---

### Installation of OpenROAD

Firstly, `or-tools` needs to be installed,

```bash
git clone https://github.com/google/or-tools.git
cd or-tools
```

then, configure the build

```bash
cmake -S . -B build -DBUILD_DEPS=ON
```

and install the `or-tools` using,

```bash
cmake --build build --config Release --target install -v
```

---

Then to set OpenROAD up,

- Clone the official **OpenROAD repository**.

```bash
git clone --recursive git clone https://github.com/The-OpenROAD-Project/OpenROAD.git
cd OpenROAD
```

- Initialize and update its submodules to include all dependencies.

```bash
git submodule update --init --recursive
```

- Compile the source code to generate the OpenROAD binaries.

```bash
rm -rf build
mkdir build
cd build
sudo cmake .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_FLAGS="-Wno-error" -DCMAKE_PREFIX_PATH="/usr/local" -DCMAKE_CXX_COMPILER=/usr/bin/g++-9
```

---

### Installation of Flow Scripts

Clone the [OpenROAD Flow Scripts repo](https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts) inside a folder of your choice

```bash 
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
```

Then to map the installed OpenROAD, and previously installed OpenSTA and Yosys, simply following the commands below,

```bash
export OPENROAD_EXE=/usr/local/bin/openroad
export YOSYS_EXE=~/Documents/Apps/oss-cad-suite/bin/yosys
export OPENSTA_EXE=/usr/local/bin/sta
```

> [!Tip]
> Use `whereis` command to locate the binaries of OpenROAD, Yosys and OpenSTA


With this OpenROAD and its script flow installation is complete.

---

## Floorplan of `gcd` 

Navigate to the `flow` directory inside `OpenROAD-flow-scripts` 

This is where the flow of our required module `gcd` will take place 

To run floorplan of this module, follow the below commands 

```bash
make DESIGN_CONFIG=./designs/sky130hd/gcd/config.mk floorplan
```

<img width="1920" height="922" alt="Screenshot from 2025-10-25 23-40-33" src="https://github.com/user-attachments/assets/da19b4ec-3501-4384-b2a4-56e10e9ce412" />

then, to view the gui,

```bash
make DESIGN_CONFIG=./designs/sky130hd/gcd/config.mk gui_floorplan
```

<img width="1920" height="922" alt="Screenshot from 2025-10-25 23-40-59" src="https://github.com/user-attachments/assets/9800fc23-326c-4c37-8558-201ba4e47430" />


---

## Placement of `gcd`

To run placement for this module, use the following command:  

```bash
make DESIGN_CONFIG=./designs/sky130hd/gcd/config.mk place
```  

<img width="1920" height="922" alt="Screenshot from 2025-10-25 23-54-08" src="https://github.com/user-attachments/assets/1aa182ef-03c5-4b61-9ea8-3ddda1e00f45" />


This will generate the placement for the module.  

To view the placement in the GUI, run:  

```bash
make DESIGN_CONFIG=./designs/sky130hd/gcd/config.mk gui_place
```

---

## Summary

This lab demonstrates the **physical design stages**—**Floorplan** and **Placement**—for the **GCD** module using **OpenROAD Flow Scripts**, transitioning from logic-level design to physical layout in the VLSI backend flow.

### Key Steps Covered:

1. **Setup and Installation**
   - Installed required tools: **OpenROAD**, **Yosys**, **OpenSTA**, and **or-tools**.
   - Ensured development environment on Ubuntu/Zorin OS with Git, CMake, GCC, and Python 3.6+.
   - Built and configured **OpenROAD** from source.
   - Cloned and configured **OpenROAD Flow Scripts**, linking installed binaries.

2. **Floorplanning**
   - Navigated to the `flow` directory inside `OpenROAD-flow-scripts`.
   - Generated floorplan 
   - Visualized the floorplan in GUI 

3. **Placement**
   - Ran placement 
   - Viewed placement in GUI 
   - Verified cell placement in the design (e.g., Sky130 standard cells).

### Outcome

By completing these steps, the **GCD module** is physically floorplanned and placed, forming the foundation for **routing and timing analysis** in subsequent VLSI backend stages.

---
