# Floorplan and Placement of GCD 

This lab focuses on using **OpenROAD Flow Scripts** to carry out the **Floorplan** and **Placement** stages for the **GCD** circuit. It demonstrates the transition from logic-level design to the **physical layout** stage in the VLSI backend flow.

---

## Table of Contents

1. [Floorplan and Placement of GCD](#floorplan-and-placement-of-gcd)
2. [Installation of OpenROAD Flow](#installation-of-openroad-flow)
   - [Installation of OpenROAD](#installation-of-openroad)
   - [Installation of Flow Scripts](#installation-of-flow-scripts)
3. [Floorplan of `gcd`](#floorplan-of-gcd)
4. [Placement of `gcd`](#placement-of-gcd)
5. [Summary](#summary-floorplan-and-placement-of-gcd-using-openroad-flow-scripts)

---

## Installation of OpenROAD Flow

Before setting up **OpenROAD Flow Scripts**, ensure the following requirements are met:

- **Operating System:** Ubuntu 20.04 or later (preferably Ubuntu 22.04) [I'm using `Zorin OS` which is based on Ubuntu 22.04]
- **Git:** Required for cloning repositories.
- **Build Tools:** Includes CMake, GCC/G++, Make, and standard development libraries needed for compiling the flow.
- **Python 3:** Version 3.6 or higher is recommended.
- **EDA Tools:**
  - **OpenROAD** – for placement and routing.
  - **Yosys** – for logic synthesis.
  - **OpenSTA** – for static timing analysis.


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
